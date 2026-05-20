# LapKr · Dashboard

Dashboard unificado del equipo LapKr. Login único + 4 tarjetas (Bitácora, Pista, Telemetry, Presiones) con permisos por usuario.

## Cómo funciona

- **Login con Google** (Firebase Auth) — único proveedor habilitado.
- **Doc de equipo unificado** en Firestore (`config/lapkr-team`) con `members: { email: { rol, tarjetas, ... } }`.
- **Roles:**
  - `admin` — acceso a las 4 tarjetas siempre, puede gestionar al equipo (agregar, quitar, cambiar rol, cambiar permisos).
  - `worker` — solo ve las tarjetas que su admin le marcó.
- **Tap a una tarjeta** redirige a la app correspondiente en su URL existente.

## Deploy

1. **Reglas Firestore** — pegar `firestore.rules` en Firebase Console → Firestore → Reglas → Publicar.
2. **Vercel** — sube este folder a un repo y conéctalo a Vercel. El dominio sugerido es `lapkr.vercel.app` o `lapkr-app.vercel.app`.
3. **Dominio autorizado** — Firebase Console → Authentication → Configuración → Dominios autorizados → agregar el dominio de Vercel.
4. **Primer login** — entrar con `ricrodsan37@gmail.com` (bootstrap admin). El doc `config/lapkr-team` se crea automáticamente la primera vez.
5. **Agregar miembros** — desde el modal "👥 Equipo" en el dashboard.

## Bootstrap admin

El email definido en la constante `BOOTSTRAP_ADMIN_EMAIL` (dentro de `index.html`) es el único que puede inicializar el doc del equipo si aún no existe. Una vez creado, los admins se gestionan desde la UI.

Para cambiar el bootstrap admin, editar la línea:
```js
const BOOTSTRAP_ADMIN_EMAIL = "ricrodsan37@gmail.com";
```

## Apps que el dashboard apunta

| Tarjeta | URL |
|---|---|
| Bitácora | `https://lapkr-bitacora.vercel.app` |
| Pista | `https://lapkr-pista.vercel.app` |
| Telemetry | `https://lapkr-telemetry.vercel.app` |
| Presiones | `https://lapkr-presiones.vercel.app` |

Si alguna URL cambia, editar el array `CARDS` dentro de `index.html`.

## Estado actual

- ✅ Login + permisos + gestión de equipo
- ❌ Botón "← Dashboard" dentro de las 4 apps (se hace en cada ronda de migración)
- ❌ Guard de permisos por tarjeta dentro de las apps (idem)
- ❌ Sistema de aprobaciones para workers (post-Ronda 1)

## Stack

- HTML/CSS/JS puro, sin frameworks
- Firebase Auth (Google) + Firestore
- Mobile-first, dark theme (negro `#0A0A0A` + azul `#2D7FF9`)
