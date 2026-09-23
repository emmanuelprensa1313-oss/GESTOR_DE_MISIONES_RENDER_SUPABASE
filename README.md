# GESTOR DE MISIONES — Electron + Render + Supabase

Esta versión conserva la aplicación Windows Electron y conecta el backend a **Supabase Postgres + Supabase Storage**. El servidor Node/Express está preparado para desplegarse en **Render**.

## Qué incluye
- Aplicación Windows con Electron.
- Login y registro.
- Roles miembro/admin y dueño.
- Usuarios y logs centralizados.
- Catálogo central de misiones.
- Subida de la carpeta completa de una misión desde el administrador.
- Almacenamiento de la misión como ZIP privado en Supabase Storage.
- Descarga de la misión en el PC del usuario al pulsar `USAR`.
- Búsqueda recursiva de `.exe`.
- Si hay varios `.exe`, el usuario puede elegir cuál ejecutar.
- Las misiones originales siguen empaquetadas dentro del instalador.
- Contraseñas almacenadas como hashes bcrypt; no se guardan en texto plano.

## Desarrollo local
Requiere Node.js LTS y Windows.

```bash
npm install
npm start
```

Para levantar el servidor local necesitas configurar las variables de Supabase y ejecutar:

```bash
npm run server
```

## Crear el instalador

```bash
npm install
npm run build
```

El instalador y el portable aparecen en `dist/`.

## Servidor en Render
Usa `GUÍA_RENDER_SUPABASE.md` y `render.yaml`.

Variables necesarias en Render:

```text
ADMIN_USER=lapara18TG
ADMIN_PASSWORD=TU_CONTRASEÑA_SEGURA
JWT_SECRET=UNA_CLAVE_LARGA_Y_ALEATORIA
SUPABASE_URL=https://TU-PROYECTO.supabase.co
SUPABASE_SERVICE_ROLE_KEY=TU_SERVICE_ROLE_KEY
SUPABASE_BUCKET=missions
```

El servidor escucha `PORT` proporcionado por Render.

## Supabase
Ejecuta `server/schema.sql` en Supabase SQL Editor. El backend crea automáticamente el bucket privado `missions` si no existe.

## Seguridad
- La `SUPABASE_SERVICE_ROLE_KEY` solo debe estar en Render.
- Nunca la pongas en `client/index.html`, `electron/main.js`, `.env` incluido en el instalador, GitHub público o capturas.
- Los clientes solo reciben una URL firmada temporal para descargar el ZIP de una misión.
- Las contraseñas usan bcrypt.
- La autenticación del API usa JWT.

## Importante sobre las misiones
El servidor recibe un ZIP de la carpeta completa, no solo el `.exe`. Esto permite conservar DLL, configuraciones y subcarpetas necesarias para ejecutar la misión.
