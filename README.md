# NicSys Finance V28 · Gemini

Integración de Gemini mediante Firebase AI Logic en el asistente de voz y la cámara existentes. Las consultas usan los módulos locales de Billetera, Finanzas, Haberes y Deudas de terceros. Los gastos, ingresos y operaciones compatibles solo quedan preparados para revisión y confirmación en la app; el modelo no escribe en Firestore.

La cámara conserva QR y OCR local. En comprobantes de una sola imagen también envía la foto a Gemini para análisis; la app no la almacena. Verificá los campos antes de registrar.

> **Actualización de una instalación existente:** para publicar NicSys Finance usá `GUIA-PUBLICACION-SEGURA.md` y el comando `firebase deploy --only hosting --project nell-app-finance`. No ejecutes el comando general `firebase deploy` de la guía de instalación inicial sobre el proyecto con datos existentes: también publicaría reglas e índices.
# Nell App Finance — guía paso a paso

Esta carpeta es un **proyecto nuevo y separado**. No toca ni comparte nada con
"Nell App Manicure". Vas a crear un proyecto de Firebase distinto y (si querés)
un repositorio de GitHub distinto.

Al final vas a tener: una web instalable de verdad (con el cartelito de
"Instalar" en Chrome/Android y "Agregar a pantalla de inicio" en iPhone),
con tus datos guardados en tu propia base de datos, protegida con usuario y
contraseña reales.

---

## Parte 1 — Crear el proyecto de Firebase

1. Andá a https://console.firebase.google.com y entrá con tu cuenta de Google
   (la misma que usás para el otro proyecto está bien, esto crea uno nuevo
   independiente).
2. **"Agregar proyecto"** → nombre: `Nell App Finance` → seguí los pasos
   (podés desactivar Google Analytics, no lo necesitás) → **Crear proyecto**.
3. En el menú de la izquierda: **Compilación → Firestore Database** →
   **Crear base de datos** → elegí una ubicación cercana (por ejemplo
   `southamerica-east1`) → modo **producción** → Habilitar.
4. En el menú de la izquierda: **Compilación → Authentication** →
   **Comenzar** → pestaña **Sign-in method** → habilitá **Correo
   electrónico/contraseña**.
5. Andá a la pestaña **Users** (Usuarios) → **Agregar usuario** → cargá tu
   email y una contraseña segura. **Ese va a ser tu usuario para entrar a la
   app** (no hay pantalla de registro dentro de la app, a propósito, para que
   nadie más pueda crear una cuenta).
6. Volvé a **Configuración del proyecto** (el ⚙ al lado de "Descripción
   general del proyecto") → bajá hasta **"Tus apps"** → tocá el ícono `</>`
   (Web) → nombre: `nelapp-web` → **NO** marques "Configurar Firebase
   Hosting" ahí (lo hacemos por CLI más abajo) → **Registrar app**.
7. Te va a mostrar un bloque de código con `const firebaseConfig = {...}`.
   **Copiá esos valores.**

## Parte 2 — Completar la configuración en los archivos

1. Abrí `public/index.html` con cualquier editor de texto (Bloc de notas
   sirve, o mejor VS Code).
2. Buscá este bloque (cerca del principio del `<script>`):
   ```js
   const firebaseConfig = {
     apiKey: "REEMPLAZAR",
     authDomain: "REEMPLAZAR.firebaseapp.com",
     projectId: "REEMPLAZAR",
     storageBucket: "REEMPLAZAR.appspot.com",
     messagingSenderId: "REEMPLAZAR",
     appId: "REEMPLAZAR"
   };
   ```
3. Reemplazá cada valor por el que copiaste en el paso 7 de la Parte 1.
   Guardá el archivo.
4. Abrí `.firebaserc` y reemplazá `PONE-ACA-EL-ID-DE-TU-PROYECTO` por el
   **ID del proyecto** (lo ves en Configuración del proyecto → "ID del
   proyecto", es como `nell-app-finance-xxxxx`).

## Parte 3 — Instalar las herramientas (una sola vez en tu computadora)

Necesitás [Node.js](https://nodejs.org) instalado (cualquier versión LTS).
Después, en una terminal:

```bash
npm install -g firebase-tools
firebase login
```

Esto abre el navegador para que inicies sesión con la misma cuenta de Google
del Parte 1.

## Parte 4 — Subir las reglas de seguridad y desplegar

Parado con la terminal **dentro de esta carpeta** (`nelapp-pwa`):

```bash
firebase deploy
```

Esto sube Firestore (las reglas de seguridad) y Hosting (la app) juntos.
Al final te va a mostrar una URL parecida a:

```
Hosting URL: https://nell-app-finance-xxxxx.web.app
```

**Esa es tu app instalable.** Abrila desde el celular en Chrome o Safari.

## Parte 5 — Importar tus datos actuales (para no perder nada)

1. Abrí tu app actual (la de Claude) y tocá el botón **⬇️** arriba de todo →
   **"Exportar datos"** → esperá a que arme el archivo → **"📋 Copiar todo"**.
2. Abrí la URL nueva de Firebase (Parte 4) en el celular o la computadora.
3. En la pantalla de login, tocá **"¿Primera vez? Importar datos de la app
   anterior"**.
4. **Primero iniciá sesión** con el usuario que creaste en el Parte 1, paso 5
   (hay un campo de email/contraseña arriba de esa pantalla; si no lo ves,
   volvé con "Volver a iniciar sesión", entrá, y de nuevo tocá el link de
   importar).
5. Pegá el texto que copiaste en el paso 1, tocá **Importar**.

Con eso quedan tus tarjetas, movimientos, horas, ventas, préstamos y demás,
tal cual estaban.

## Parte 6 — Instalarla en el celular

- **Android (Chrome):** abrí la URL → menú (⋮) → **Instalar aplicación**.
  Ahora sí va a funcionar de verdad, porque esta versión tiene el archivo
  `manifest.json` y el `service-worker.js` que Chrome pide para instalar.
- **iPhone (Safari):** abrí la URL → botón compartir (□↑) → **Agregar a
  pantalla de inicio**.

## Parte 7 (opcional) — Guardarlo en GitHub, como proyecto nuevo y separado

1. Andá a https://github.com/new
2. Nombre del repositorio: `nell-app-finance` (o el que quieras).
3. **No** marques "Initialize with README" (ya tenés uno).
4. Creá el repositorio.
5. En la terminal, dentro de esta carpeta:
   ```bash
   git init
   git add .
   git commit -m "Nell App Finance - primera versión"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/nell-app-finance.git
   git push -u origin main
   ```
   (Reemplazá `TU_USUARIO` por tu usuario de GitHub.)

Esto es un repositorio **completamente nuevo**, no toca el de Nell App
Manicure para nada.

### Actualizaciones futuras

Cada vez que yo te pase una nueva versión de `public/index.html` (por algún
cambio que pidas), la reemplazás en esta carpeta y corrés de nuevo:

```bash
firebase deploy
```

Eso actualiza la app ya instalada — la próxima vez que la abras en el
celular, va a tomar los cambios solos (gracias al service worker).

---

## Resumen de qué es cada archivo

- `public/index.html` — la app completa (mismo diseño y funciones de siempre).
- `public/manifest.json` — le dice al celular el nombre, ícono y colores para
  instalarla.
- `public/service-worker.js` — permite que Chrome la reconozca como
  instalable y funcione un poco offline.
- `public/icons/` — los íconos reales de NelApp en varios tamaños.
- `firebase.json` / `firestore.rules` / `firestore.indexes.json` /
  `.firebaserc` — configuración de tu proyecto de Firebase.

## Seguridad

- Solo quien tenga el usuario y contraseña que creaste en Firebase
  Authentication puede entrar y ver tus datos.
- No hay pantalla de "crear cuenta" dentro de la app, así que nadie más
  puede registrarse solo.
- Los datos viajan siempre por HTTPS.
