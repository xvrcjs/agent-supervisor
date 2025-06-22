# Agent Supervisor – Monitoreo de Uso de Celulares

## Project Overview
Agent Supervisor es un MVP Android y web que monitorea el uso de YouTube mediante un agente de IA. El sistema captura pantallas del dispositivo, analiza su contenido y, si detecta parámetros no permitidos, notifica al tutor por WhatsApp o emite una alerta.

```
[Flutter Web/Android] <---> [Django REST API] <---> [PostgreSQL]
```

## Directory Structure
- **/backend** – API REST con Django y DRF
- **/frontend** – Aplicación Flutter (web para pruebas)
- **/db** – Directorio de datos de PostgreSQL
- **docker-compose.yml** – Orquestación de servicios
- **.env.example** – Variables de entorno de ejemplo

## Prerequisitos
- Docker y Docker Compose
- Flutter SDK (para pruebas en Android)
- Android Studio o emulador configurado

## Configuración del Entorno
1. Copia `.env.example` a `.env` y edita los valores según tu entorno.
2. Asegúrate de que las variables de la base de datos y Django coincidan con tu configuración.

## Construcción y Ejecución
```bash
docker-compose up --build
```
- API disponible en [http://localhost:8000](http://localhost:8000)
- Interfaz Flutter web en [http://localhost:8080](http://localhost:8080)

## Pruebas en Android
```bash
cd frontend
flutter pub get
flutter run --debug # especifica dispositivo si es necesario
```
Actualiza la URL base de la API en tu aplicación para apuntar a `http://localhost:8000` cuando pruebes en el emulador o dispositivo.

## Gestión de la Base de Datos
Puedes acceder al contenedor de PostgreSQL con:
```bash
docker exec -it $(docker compose ps -q db) psql -U $POSTGRES_USER $POSTGRES_DB
```
Ejecuta migraciones manualmente si lo requieres:
```bash
docker compose exec backend python manage.py migrate
```

## Solución de Problemas
- **Variables de entorno faltantes:** verifica tu archivo `.env`.
- **Puertos en uso:** asegúrate de que 5432, 8000 y 8080 estén libres.

## Comandos Útiles
- `docker compose logs -f backend` – Ver logs del backend
- `docker compose logs -f frontend` – Ver logs del frontend
- `docker compose logs -f db` – Ver logs de PostgreSQL

## Contribución y Licencia
Las contribuciones son bienvenidas mediante issues y pull requests. Este proyecto se distribuye bajo la licencia MIT.
