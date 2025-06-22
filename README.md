# Agent Supervisor – Monitoreo de Uso de Celulares

## Objetivo
Desarrollar un agente IA móvil que capture periódicamente pantallazos de YouTube en el dispositivo del niño, analice el contenido y, si detecta parámetros fuera de lo permitido, notifique al tutor vía WhatsApp o active una alerta sonora.

## Tecnologías y Stack
- **Backend:** Python 3.11, Django 4.x, Django REST Framework  
- **Base de Datos:** PostgreSQL 15  
- **Frontend:** Dart, Flutter (Android MVP + web)  
- **Contenerización:** Docker & Docker Compose  
- **Comunicación:** HTTP/REST  
- **Servicios Adicionales:** Redis, RabbitMQ (o Celery+Redis), Elasticsearch (opcional)

## Estructura de Carpetas
```bash
/backend    # Código Django + DRF
/frontend   # Código Flutter
/infra      # docker-compose.yml, scripts y configuraciones
```

## Levantamiento
```bash
# Desde la raíz del repositorio:
docker-compose up --build -d
```

### Variables de entorno obligatorias
- `POSTGRES_USER`  
- `POSTGRES_PASSWORD`  
- `POSTGRES_DB`  
- `DB_HOST`  
- `DB_PORT`  
- `DJANGO_SECRET_KEY`  
- `DJANGO_DEBUG`  
- `FLUTTER_BUILD_MODE`  
- `WHATSAPP_API_TOKEN`  
- `ALLOWED_YOUTUBE_CATEGORIES`

## Convenciones y Estándares
- **Ramas:**  
  - `main` (producción)  
  - `develop` (integración de features)  
  - `feature/<nombre-corto>`  
  - `hotfix/<nombre-corto>`  
- **Lint / Formateo:**  
  - Backend: Black, isort, Flake8  
  - Frontend: `dart format`, `dart analyze`  
  - Dockerfiles & YAML: hadolint, yamllint  
- **CI/CD (GitHub Actions):**  
  - `ci.yml` en `pull_request` y `push` a `develop/feature/*`  
  - `cd.yml` en `push` a `main` con build, test, publicación y despliegue
