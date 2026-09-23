# GESTOR DE MISIONES — Electron + servidor + archivo JSON

Esta versión convierte el proyecto HTML en una aplicación Windows con Electron. La interfaz original se conserva y se añade un backend central con Express + archivo JSON.

## 1. Instalar
Requiere Node.js LTS y Windows.

```bash
npm install
npm start
```

## 2. Servidor central
En el PC/VPS que funcionará como servidor:

```bash
npm install
set ADMIN_PASSWORD=TU_CLAVE_SEGURA
set JWT_SECRET=UNA_CLAVE_LARGA
npm run server
```

El servidor escucha en el puerto 8787. Para varios PCs, usa la IP/dominio del servidor, por ejemplo `http://192.168.1.50:8787`.

## 3. Conectar el cliente al servidor
La aplicación guarda la URL en `%APPDATA%/Gestor de Misiones/config.json`.
En desarrollo puedes arrancar con:

```bash
set GM_SERVER_URL=http://192.168.1.50:8787
npm start
```

## 4. Crear el .exe

```bash
npm run build
```

El instalador y el portable aparecen en `dist/`.

## 5. Misiones
Las misiones originales están en `resources/TODAS LAS MISIONES`. Electron puede ejecutar `.exe` localmente, algo que un HTML puro no puede hacer.

Las misiones añadidas desde el cliente se copian a la carpeta de datos de ese PC y solo se registran en el servidor como metadatos. Para distribuir automáticamente nuevas carpetas de misión a todos los PCs hace falta añadir almacenamiento de archivos al servidor (S3/MinIO/NAS o un endpoint de subida/descarga). La base de datos ya está preparada para guardar el catálogo central.

## Seguridad
Las contraseñas se almacenan con bcrypt y el servidor entrega JWT. No se guardan contraseñas en texto plano en la base de datos.


## CORRECCIÓN DE INSTALACIÓN DE WINDOWS

Esta versión ya NO utiliza `better-sqlite3`. Por eso `npm install` no necesita
compilar `node-gyp` ni instalar Visual Studio Build Tools para la base de datos.

Ejecuta:
1. `npm install`
2. `npm run server` (en una ventana)
3. `npm start` (en otra ventana)
4. Para crear el instalador: `npm run build`

El instalador se generará dentro de `dist`.

La base de datos del servidor se guarda en `server/data/gestor-data.json`.
Las contraseñas se guardan como hashes bcrypt y no en texto plano.
