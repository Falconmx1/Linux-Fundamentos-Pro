```markdown
# Docker Compose

## 📘 ¿Qué es Docker Compose?

**Docker Compose** es una herramienta para definir y ejecutar aplicaciones multi-contenedor con un solo archivo YAML.

---

## 📝 Sintaxis de docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - mi-red
    depends_on:
      - db

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: app
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - mi-red

volumes:
  db_data:

networks:
  mi-red:
🛠️ Comandos de Docker Compose

# Iniciar todos los servicios
docker-compose up

# Iniciar en segundo plano
docker-compose up -d

# Detener servicios
docker-compose down

# Ver logs
docker-compose logs
docker-compose logs -f web

# Ejecutar comando en servicio
docker-compose exec web bash

# Reconstruir
docker-compose build
docker-compose up -d --build

# Escalar servicios
docker-compose up -d --scale web=3

# Ver estado
docker-compose ps
docker-compose top
🧪 Ejemplo: WordPress con Docker Compose
yaml
# docker-compose.yml
version: '3.8'

services:
  db:
    image: mysql:8.0
    container_name: wordpress-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - wordpress-network

  wordpress:
    image: wordpress:latest
    container_name: wordpress-app
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress_data:/var/www/html
    networks:
      - wordpress-network
    depends_on:
      - db

volumes:
  db_data:
  wordpress_data:

networks:
  wordpress-network:
    driver: bridge
Comandos

# Iniciar WordPress
docker-compose up -d

# Acceder a WordPress
# http://localhost:8080

# Ver logs
docker-compose logs -f

# Detener
docker-compose down

# Detener y eliminar volúmenes
docker-compose down -v
📊 Redes en Docker Compose
yaml
# Redes personalizadas
services:
  web:
    networks:
      - frontend
      - backend
  
  app:
    networks:
      - backend
  
  db:
    networks:
      - backend

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
    internal: true  # Red interna sin acceso externo
🧪 Script de despliegue

#!/bin/bash
# deploy-compose.sh - Despliegue con Docker Compose

APP_NAME="mi-app"
COMPOSE_FILE="docker-compose.yml"

echo "=== DESPLIEGUE DE $APP_NAME ==="

# Verificar Docker Compose
if ! command -v docker-compose &>/dev/null; then
    echo "Instalando Docker Compose..."
    sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
fi

# Descargar archivos
echo "Descargando configuración..."
curl -O https://raw.githubusercontent.com/ejemplo/app/main/docker-compose.yml

# Iniciar
echo "Iniciando servicios..."
docker-compose up -d

# Verificar
echo "✅ Despliegue completado"
echo "📋 Servicios:"
docker-compose ps

echo "📊 Logs:"
docker-compose logs --tail=10
