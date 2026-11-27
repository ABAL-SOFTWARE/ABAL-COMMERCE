# Refer API - Guía de Despliegue en Linux (Docker)

Este repositorio contiene la configuración lista para levantar **Refer API** en Linux con Docker Compose (sin código fuente).

## Requisitos (Linux)
- Docker Engine y Docker Compose v2 (`docker compose`)
- `openssl` (para generar los secretos)

## 1. Clonar el repositorio
```bash
git clone <URL_DEL_REPO>
cd <CARPETA_DEL_REPO>
```

## 2. Configurar variables de entorno
Copia el archivo de ejemplo y genera los secretos obligatorios directamente en `.env`:
```bash
cp .env.example .env
sed -i.bak \
  -e "s|^DB_PASSWORD=.*|DB_PASSWORD=$(openssl rand -hex 16)|" \
  -e "s|^AUTH_JWT_SECRET=.*|AUTH_JWT_SECRET=$(openssl rand -base64 48 | tr -d '\n')|" \
  -e "s|^KRATOS_SECRETS_CIPHER=.*|KRATOS_SECRETS_CIPHER=$(openssl rand -hex 16)|" \
  -e "s|^KRATOS_SECRETS_COOKIE=.*|KRATOS_SECRETS_COOKIE=$(openssl rand -hex 32)|" \
  .env && rm -f .env.bak
```

Edita `.env` si necesitas ajustar `STRIPE_SECRET_KEY`, `CORS_ORIGINS`, `KRATOS_SMTP_URI` (o desactivar email OTP con `SIGNUP_EMAIL_ENABLED=false`) y `ADMIN_PASSWORD`.

## 3. Levantar el stack
```bash
docker compose up -d
```

Esto iniciará automáticamente:
- `postgres`: Base de datos PostgreSQL 16 (crea las bases `refer` y `kratos`).
- `kratos-migrate` y `kratos`: Servidor de identidad Ory Kratos.
- `app`: Refer API en el puerto configurado (`HOST_PORT`, por defecto `3001`).

## 4. Verificar funcionamiento
```bash
docker compose ps
curl http://localhost:3001/health
```

## Comandos útiles
- Ver logs: `docker compose logs -f app`
- Detener servicios: `docker compose down`
- Detener y borrar datos de base de datos: `docker compose down -v`
