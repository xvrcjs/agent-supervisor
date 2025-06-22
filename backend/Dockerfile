FROM python:3.11-slim

RUN apt-get update && apt-get install -y netcat && rm -rf /var/lib/apt/lists/*

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

RUN chmod +x wait-for-db.sh

EXPOSE 8000

CMD ["sh", "-c", "./wait-for-db.sh db 5432 && python manage.py migrate && gunicorn project.wsgi:application --bind 0.0.0.0:8000"]
