```markdown
# Fundamentos de Docker

## 📘 ¿Qué es Docker?

**Docker** es una plataforma de contenedores que permite empaquetar aplicaciones y sus dependencias en contenedores ligeros y portátiles.

---

## 🏗️ Conceptos clave

| Concepto | Descripción |
|----------|-------------|
| **Imagen** | Plantilla de solo lectura con instrucciones para crear un contenedor |
| **Contenedor** | Instancia ejecutable de una imagen |
| **Dockerfile** | Archivo con instrucciones para construir una imagen |
| **Registry** | Repositorio de imágenes (Docker Hub) |
| **Volumen** | Persistencia de datos fuera del contenedor |

---

## 🛠️ Comandos básicos de Docker

### Gestión de imágenes
```bash
# Buscar imágenes
docker search ubuntu
docker search nginx

# Descargar imagen
docker pull ubuntu:22.04
docker pull nginx:alpine

# Listar imágenes
docker images
docker image ls

# Eliminar imagen
docker rmi ubuntu:22.04
docker image prune -a
Gestión de contenedores
bash
# Ejecutar contenedor
docker run ubuntu echo "Hola Docker"
docker run -it ubuntu bash

# Ejecutar en segundo plano
docker run -d --name mi-nginx nginx

# Listar contenedores
docker ps          # En ejecución
docker ps -a       # Todos
docker ps -a -q    # IDs de todos

# Detener/Iniciar
docker stop mi-nginx
docker start mi-nginx

# Eliminar contenedor
docker rm mi-nginx
docker rm -f mi-nginx  # Forzado
docker container prune  # Todos los detenidos
Logs y ejecución

# Ver logs
docker logs mi-nginx
docker logs -f mi-nginx  # Seguimiento

# Ejecutar comando en contenedor en ejecución
docker exec -it mi-nginx bash

# Copiar archivos
docker cp archivo.txt mi-nginx:/tmp/
docker cp mi-nginx:/tmp/archivo.txt .
📝 Dockerfile básico
dockerfile
# Dockerfile
FROM ubuntu:22.04

LABEL maintainer="usuario@ejemplo.com"

RUN apt update && apt install -y nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
Construir y ejecutar

# Construir imagen
docker build -t mi-nginx .

# Ejecutar
docker run -d -p 8080:80 --name nginx-test mi-nginx
📊 Mapeo de puertos

# Mapeo simple
docker run -d -p 80:80 nginx
docker run -d -p 8080:80 nginx

# Mapeo a IP específica
docker run -d -p 127.0.0.1:8080:80 nginx

# Mapeo aleatorio
docker run -d -P nginx
🧪 Ejemplo práctico

#!/bin/bash
# docker-setup.sh - Configuración de contenedor web

# Crear directorio para web
mkdir -p ~/docker-web
cd ~/docker-web

# Crear página HTML
cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Docker Web</title></head>
<body>
    <h1>¡Hola desde Docker!</h1>
    <p>Sitio web servido desde un contenedor</p>
</body>
</html>
EOF

# Crear Dockerfile
cat > Dockerfile << 'EOF'
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
EOF

# Construir
docker build -t mi-web .

# Ejecutar
docker run -d -p 8080:80 --name web-server mi-web

# Verificar
echo "✅ Sitio web disponible en: http://localhost:8080"
curl http://localhost:8080
