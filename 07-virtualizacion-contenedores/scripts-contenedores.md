```markdown
# Scripts para Contenedores

## 🛠️ Scripts útiles para Docker

### Script 1: Limpieza de Docker
```bash
#!/bin/bash
# docker-cleanup.sh - Limpieza de recursos Docker

echo "=== LIMPIEZA DE DOCKER ==="

# Detener contenedores en ejecución
echo "Deteniendo contenedores en ejecución..."
docker stop $(docker ps -q) 2>/dev/null

# Eliminar todos los contenedores detenidos
echo "Eliminando contenedores detenidos..."
docker container prune -f

# Eliminar imágenes no utilizadas
echo "Eliminando imágenes no utilizadas..."
docker image prune -a -f

# Eliminar volúmenes no utilizados
echo "Eliminando volúmenes no utilizados..."
docker volume prune -f

# Eliminar redes no utilizadas
echo "Eliminando redes no utilizadas..."
docker network prune -f

# Ver espacio liberado
echo "Espacio en disco:"
df -h /var/lib/docker

echo "✅ Limpieza completada"
Script 2: Backup de contenedor

#!/bin/bash
# docker-backup.sh - Backup de contenedor

CONTAINER_NAME="$1"
BACKUP_DIR="/backup/docker"

if [ -z "$CONTAINER_NAME" ]; then
    echo "Uso: $0 <nombre-contenedor>"
    exit 1
fi

mkdir -p "$BACKUP_DIR"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="${BACKUP_DIR}/${CONTAINER_NAME}_${DATE}.tar"

echo "=== BACKUP DE $CONTAINER_NAME ==="

# Detener contenedor
echo "Deteniendo contenedor..."
docker stop "$CONTAINER_NAME"

# Exportar contenedor
echo "Exportando contenedor..."
docker export "$CONTAINER_NAME" > "$BACKUP_FILE"

# Iniciar contenedor
echo "Iniciando contenedor..."
docker start "$CONTAINER_NAME"

# Verificar
if [ -f "$BACKUP_FILE" ]; then
    size=$(du -h "$BACKUP_FILE" | cut -f1)
    echo "✅ Backup creado: $BACKUP_FILE ($size)"
else
    echo "❌ Error al crear backup"
    exit 1
fi
Script 3: Monitor de contenedores

#!/bin/bash
# docker-monitor.sh - Monitoreo de contenedores

echo "=== MONITOR DE CONTENEDORES ==="
echo "Fecha: $(date)"
echo

# Contenedores en ejecución
echo "▶️ Contenedores activos:"
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
echo

# Uso de recursos
echo "📊 Uso de recursos:"
docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}\t{{.NetIO}}"
echo

# Volúmenes
echo "💾 Volúmenes:"
docker volume ls
echo

# Redes
echo "🌐 Redes:"
docker network ls
Script 4: Despliegue de aplicación

#!/bin/bash
# docker-deploy.sh - Despliegue automatizado

APP_DIR="/opt/mi-app"
COMPOSE_FILE="docker-compose.yml"

deploy_app() {
    echo "=== DESPLIEGUE DE APLICACIÓN ==="
    
    # Clonar repositorio
    echo "Clonando repositorio..."
    git clone https://github.com/ejemplo/mi-app.git "$APP_DIR" 2>/dev/null || (cd "$APP_DIR" && git pull)
    
    cd "$APP_DIR" || exit 1
    
    # Construir imágenes
    echo "Construyendo imágenes..."
    docker-compose build
    
    # Detener servicios antiguos
    echo "Deteniendo servicios antiguos..."
    docker-compose down
    
    # Iniciar nuevos servicios
    echo "Iniciando servicios..."
    docker-compose up -d
    
    # Verificar
    echo "✅ Despliegue completado"
    docker-compose ps
}

rollback_deploy() {
    echo "=== ROLLBACK ==="
    docker-compose down
    docker-compose up -d --no-build
    echo "✅ Rollback completado"
}

# Menú
echo "1. Desplegar"
echo "2. Rollback"
echo "3. Estado"
read -p "Elige opción: " opcion

case $opcion in
    1) deploy_app ;;
    2) rollback_deploy ;;
    3) docker-compose -f "$APP_DIR/$COMPOSE_FILE" ps ;;
    *) echo "Opción inválida" ;;
esac
Script 5: Backup de volúmenes

#!/bin/bash
# volume-backup.sh - Backup de volumen Docker

VOLUME_NAME="$1"
BACKUP_DIR="/backup/volumes"

if [ -z "$VOLUME_NAME" ]; then
    echo "Uso: $0 <nombre-volumen>"
    echo "Volúmenes disponibles:"
    docker volume ls
    exit 1
fi

mkdir -p "$BACKUP_DIR"
DATE=$(date +%Y%m%d_%H%M%S)

echo "=== BACKUP DE VOLUMEN: $VOLUME_NAME ==="

# Crear contenedor temporal para backup
docker run --rm -v "$VOLUME_NAME:/volume" -v "$BACKUP_DIR:/backup" alpine \
    tar -czf "/backup/${VOLUME_NAME}_${DATE}.tar.gz" -C /volume .

# Verificar
BACKUP_FILE="${BACKUP_DIR}/${VOLUME_NAME}_${DATE}.tar.gz"
if [ -f "$BACKUP_FILE" ]; then
    size=$(du -h "$BACKUP_FILE" | cut -f1)
    echo "✅ Backup creado: $BACKUP_FILE ($size)"
    ls -lh "$BACKUP_DIR" | grep "$VOLUME_NAME"
else
    echo "❌ Error al crear backup"
    exit 1
fi
📋 Scripts para LXC/LXD

#!/bin/bash
# lxc-manager.sh - Gestión de contenedores LXC

# Crear contenedor
create_container() {
    local name="$1"
    local distro="${2:-ubuntu}"
    local release="${3:-22.04}"
    
    echo "Creando contenedor: $name"
    lxc launch "$distro:$release" "$name"
    
    # Instalar paquetes básicos
    lxc exec "$name" -- apt update
    lxc exec "$name" -- apt install -y curl wget vim
}

# Listar contenedores
list_containers() {
    echo "=== CONTENEDORES LXC ==="
    lxc list
}

# Backup de contenedor
backup_container() {
    local name="$1"
    echo "Creando backup de $name..."
    lxc export "$name" "/backup/${name}.tar.gz"
}

# Menú
case "$1" in
    create)
        create_container "$2" "$3" "$4"
        ;;
    list)
        list_containers
        ;;
    backup)
        backup_container "$2"
        ;;
    *)
        echo "Uso: $0 {create|list|backup} [args]"
        ;;
esac
