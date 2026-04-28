# Intranet Uni - Deploy Vercel + Render

Proyecto dividido en:
- `frontend` (React + Vite) -> Vercel
- `backend` (Express + PostgreSQL) -> Render

## Variables de entorno

### Backend (`backend/.env`)
Basado en `backend/.env.example`:

- `PORT` (Render usa `10000` por defecto)
- `DATABASE_URL` (recomendado en producción)
- `JWT_SECRET`
- `JWT_EXPIRES_IN` (ej: `8h`)
- `CORS_ORIGIN` (dominio frontend en Vercel, por ejemplo `https://tu-app.vercel.app`)

Si no usas `DATABASE_URL`, puedes usar:
- `DB_HOST`
- `DB_PORT`
- `DB_NAME`
- `DB_USER`
- `DB_PASSWORD`

### Frontend (`frontend/.env`)
Basado en `frontend/.env.example`:

- `VITE_API_URL` (ej: `https://tu-backend.onrender.com/api`)

## Deploy backend en Render

1. Conecta el repo en Render.
2. Render detectará `render.yaml` en la raíz.
3. Completa variables sensibles:
   - `DATABASE_URL`
   - `JWT_SECRET`
   - `CORS_ORIGIN` (URL exacta de Vercel, sin `/` final)
4. Despliega y valida `GET /health`.

## Deploy frontend en Vercel

1. Importa el repo en Vercel.
2. Selecciona `frontend` como Root Directory.
3. Configura `VITE_API_URL` con la URL pública de Render + `/api`.
4. Deploy.
5. Verifica rutas internas (ej: `/dashboard`, `/perfil`), ya que `frontend/vercel.json` incluye rewrite SPA a `index.html`.

## Orden recomendado de despliegue

1. Despliega backend en Render.
2. Copia URL pública de Render (ej: `https://intranet-backend.onrender.com`).
3. Configura `VITE_API_URL=https://intranet-backend.onrender.com/api` en Vercel.
4. Despliega frontend en Vercel.
5. Copia URL de Vercel y colócala en `CORS_ORIGIN` en Render.
6. Redeploy backend para aplicar CORS final.

## Verificación rápida

- Frontend carga sin errores.
- Login responde contra backend en Render.
- Backend responde `{"status":"ok"}` en `/health`.
