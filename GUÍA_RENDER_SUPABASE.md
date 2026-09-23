# Guía rápida: Render + Supabase

## 1. Crear el proyecto en Supabase
1. Crea un proyecto en Supabase.
2. Abre **SQL Editor**.
3. Copia todo `server/schema.sql` y ejecútalo.
4. Ve a **Project Settings > API**.
5. Copia:
   - Project URL -> `SUPABASE_URL`
   - `service_role` key -> `SUPABASE_SERVICE_ROLE_KEY`

**IMPORTANTE:** la `service_role` key es secreta. Nunca la pongas dentro del `.exe`, HTML o GitHub. Solo va en las variables privadas de Render.

El servidor crea automáticamente el bucket privado `missions` si todavía no existe.

## 2. Subir el servidor a Render
Render necesita el proyecto en un repositorio Git (por ejemplo GitHub).

Sube esta carpeta completa al repositorio. Luego en Render crea un **Web Service** desde ese repositorio.

Puedes usar `render.yaml` como referencia. Si Render pregunta manualmente:

- Build Command: `npm install`
- Start Command: `npm run server`
- Health Check Path: `/api/health`

Variables privadas:

```text
ADMIN_USER=lapara18TG
ADMIN_PASSWORD=TU_CONTRASEÑA_SEGURA
JWT_SECRET=UNA_CLAVE_LARGA_Y_ALEATORIA
SUPABASE_URL=https://TU-PROYECTO.supabase.co
SUPABASE_SERVICE_ROLE_KEY=TU_SERVICE_ROLE_KEY
SUPABASE_BUCKET=missions
```

Cuando Render termine, tendrás una URL parecida a:

```text
https://gestor-de-misiones-api.onrender.com
```

Prueba:

```text
https://gestor-de-misiones-api.onrender.com/api/health
```

Debe responder JSON con `ok: true`.

## 3. Conectar el .exe
En la PC que genera el instalador, antes de compilar puedes usar la variable:

```bat
set GM_SERVER_URL=https://TU-SERVICIO.onrender.com
npm run build
```

La aplicación también puede guardar la URL en `%APPDATA%/Gestor de Misiones/config.json` mediante la API de configuración existente.

## 4. Flujo de misiones
- Las misiones incluidas originalmente siguen dentro del instalador.
- Un administrador puede seleccionar una carpeta completa.
- Electron copia la carpeta completa al PC del administrador.
- Solo se usan los `.exe` para detectar los ejecutables.
- Al ser administrador, Electron comprime la carpeta completa y la sube al servidor.
- El servidor guarda el ZIP en el bucket privado de Supabase Storage y guarda los metadatos en Supabase Postgres.
- Otro usuario ve la misión en su catálogo.
- Al pulsar `USAR`, el cliente descarga el ZIP mediante una URL firmada temporal, lo guarda localmente, busca todos los `.exe` y permite ejecutar el seleccionado.

## 5. Límites del plan gratuito
Los servicios gratuitos de terceros tienen límites de almacenamiento, transferencia, CPU y/o suspensión. Revisa los límites actuales de Render y Supabase antes de distribuir muchas misiones grandes.
