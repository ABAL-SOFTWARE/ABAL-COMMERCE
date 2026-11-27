# Refer Web - Guía de Despliegue en Linux (Docker)

Esta carpeta contiene la configuración lista para levantar **Refer Web** (panel de administración Angular) en Linux con Docker Compose (sin código fuente).

## Requisitos (Linux)
- Docker Engine y Docker Compose v2 (`docker compose`)

## 1. Clonar el repositorio
```bash
git clone <URL_DEL_REPO>
cd <CARPETA_DEL_REPO>/frontend
```

## 2. Configurar variables de entorno
Copia el archivo de ejemplo y ajusta las URLs si es necesario:
```bash
cp .env.example .env
```

Variables principales en `.env`:
- `HOST_PORT`: puerto expuesto en el host (por defecto `4200`).
- `APP_BACKEND_URL`: URL pública del backend (por defecto `http://localhost:3001`).
- `APP_API_PATH`: prefijo de la API REST (por defecto `/api/v1`).
- `APP_API_URL`: opcional; si se define, sobreescribe `APP_BACKEND_URL + APP_API_PATH`.
- `APP_BASE_URL`: URL pública del frontend (por defecto `http://localhost:4200`).

> Recuerda incluir el dominio del frontend en `CORS_ORIGINS` del backend (`backend/.env`).

## 3. Levantar el servicio
```bash
docker compose up -d
```

## 4. Verificar funcionamiento
```bash
docker compose ps
curl http://localhost:4200/health
```

## Comandos útiles
- Ver logs: `docker compose logs -f web`
- Detener servicio: `docker compose down`
