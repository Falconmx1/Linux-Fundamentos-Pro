```markdown
# Ejercicios Prácticos - Bash Scripting

## 🎯 Ejercicio 1: Script de sistema

```bash
#!/bin/bash
# monitor.sh - Monitorea el sistema

echo "=== MONITOR DEL SISTEMA ==="
echo "Fecha: $(date)"
echo "Uptime: $(uptime -p)"
echo "CPU: $(top -bn1 | grep 'Cpu(s)' | awk '{print $2}')%"
echo "Memoria: $(free -h | grep Mem | awk '{print $3 "/" $2}')"
echo "Disco: $(df -h / | awk 'NR==2 {print $3 "/" $2 " (" $5 ")"}')"
echo "Procesos: $(ps aux | wc -l)"
echo "==========================="
🎯 Ejercicio 2: Organizador de archivos

#!/bin/bash
# organizar.sh - Organiza archivos por extensión

# Definir directorios por extensión
declare -A tipos
tipos=(
    [".txt"]="textos"
    [".pdf"]="documentos"
    [".jpg"]="imagenes"
    [".png"]="imagenes"
    [".mp3"]="musica"
    [".mp4"]="videos"
    [".zip"]="comprimidos"
    [".gz"]="comprimidos"
    [".sh"]="scripts"
)

# Crear directorios y mover archivos
for archivo in *; do
    if [ -f "$archivo" ]; then
        extension=".${archivo##*.}"
        destino="${tipos[$extension]}"
        
        if [ -n "$destino" ]; then
            mkdir -p "$destino"
            mv "$archivo" "$destino/"
            echo "Movido: $archivo → $destino/"
        fi
    fi
done
🎯 Ejercicio 3: Gestor de usuarios

#!/bin/bash
# usuarios.sh - Gestión básica de usuarios

color_ok() { echo -e "\033[32m[✔] $1\033[0m"; }
color_error() { echo -e "\033[31m[✘] $1\033[0m" >&2; }
color_info() { echo -e "\033[34m[→] $1\033[0m"; }

crear_usuario() {
    local usuario="$1"
    
    if id "$usuario" &>/dev/null; then
        color_error "El usuario $usuario ya existe"
        return 1
    fi
    
    sudo useradd -m -s /bin/bash "$usuario"
    sudo passwd "$usuario"
    color_ok "Usuario $usuario creado exitosamente"
}

eliminar_usuario() {
    local usuario="$1"
    
    if ! id "$usuario" &>/dev/null; then
        color_error "El usuario $usuario no existe"
        return 1
    fi
    
    sudo userdel -r "$usuario"
    color_ok "Usuario $usuario eliminado"
}

listar_usuarios() {
    echo "=== USUARIOS DEL SISTEMA ==="
    cut -d: -f1 /etc/passwd | sort
}

# Menú principal
while true; do
    echo
    echo "=== GESTOR DE USUARIOS ==="
    echo "1. Crear usuario"
    echo "2. Eliminar usuario"
    echo "3. Listar usuarios"
    echo "4. Salir"
    read -p "Elige una opción: " opcion
    
    case $opcion in
        1)
            read -p "Nombre de usuario: " usuario
            crear_usuario "$usuario"
            ;;
        2)
            read -p "Nombre de usuario: " usuario
            eliminar_usuario "$usuario"
            ;;
        3)
            listar_usuarios
            ;;
        4)
            echo "¡Hasta luego!"
            exit 0
            ;;
        *)
            color_error "Opción inválida"
            ;;
    esac
done
🎯 Ejercicio 4: Monitor de logs

#!/bin/bash
# log-monitor.sh - Monitorea logs en tiempo real

LOG_FILE="/var/log/syslog"
INTERVALO=5

if [ ! -f "$LOG_FILE" ]; then
    echo "Error: El archivo de log no existe"
    exit 1
fi

echo "Monitoreando $LOG_FILE (Ctrl+C para salir)"

tail -f "$LOG_FILE" | while read -r linea; do
    if echo "$linea" | grep -qi "error\|fail\|critical"; then
        echo -e "\033[31m[ERROR]\033[0m $linea"
    elif echo "$linea" | grep -qi "warning\|warn"; then
        echo -e "\033[33m[WARN]\033[0m $linea"
    else
        echo -e "\033[34m[INFO]\033[0m $linea"
    fi
done
⭐ Desafío final: Script de respaldo automático

#!/bin/bash
# backup-auto.sh - Sistema de respaldo automático

CONFIG_DIR="$HOME/.backup_config"
mkdir -p "$CONFIG_DIR"

# Configuración por defecto
ORIGEN="$HOME/documentos"
DESTINO="$HOME/backups"
RETENCION=7  # días

# Cargar configuración si existe
if [ -f "$CONFIG_DIR/config" ]; then
    source "$CONFIG_DIR/config"
fi

# Funciones
crear_backup() {
    local fecha=$(date +%Y%m%d_%H%M%S)
    local archivo="backup_${fecha}.tar.gz"
    local ruta_completa="${DESTINO}/${archivo}"
    
    mkdir -p "$DESTINO"
    
    echo "Creando backup de $ORIGEN..."
    tar -czf "$ruta_completa" -C "$(dirname "$ORIGEN")" "$(basename "$ORIGEN")"
    
    if [ $? -eq 0 ]; then
        echo "✅ Backup creado: $ruta_completa"
        echo "Tamaño: $(du -h "$ruta_completa" | cut -f1)"
        limpiar_backups_viejos
    else
        echo "❌ Error al crear el backup"
        return 1
    fi
}

limpiar_backups_viejos() {
    echo "Limpiando backups de más de ${RETENCION} días..."
    find "$DESTINO" -name "backup_*.tar.gz" -mtime +$RETENCION -delete
}

mostrar_estado() {
    echo "=== ESTADO DE BACKUPS ==="
    echo "Origen: $ORIGEN"
    echo "Destino: $DESTINO"
    echo "Retención: $RETENCION días"
    echo "Backups existentes:"
    ls -lh "$DESTINO" 2>/dev/null || echo "  (no hay backups)"
}

configurar() {
    read -p "Directorio origen [$ORIGEN]: " new_origen
    [ -n "$new_origen" ] && ORIGEN="$new_origen"
    
    read -p "Directorio destino [$DESTINO]: " new_destino
    [ -n "$new_destino" ] && DESTINO="$new_destino"
    
    read -p "Días de retención [$RETENCION]: " new_retencion
    [ -n "$new_retencion" ] && RETENCION="$new_retencion"
    
    # Guardar configuración
    cat > "$CONFIG_DIR/config" << EOF
ORIGEN="$ORIGEN"
DESTINO="$DESTINO"
RETENCION=$RETENCION
EOF
    echo "✅ Configuración guardada"
}

# Menú principal
while true; do
    echo
    echo "=== SISTEMA DE BACKUPS ==="
    echo "1. Crear backup"
    echo "2. Ver estado"
    echo "3. Configurar"
    echo "4. Salir"
    read -p "Elige una opción: " opcion
    
    case $opcion in
        1) crear_backup ;;
        2) mostrar_estado ;;
        3) configurar ;;
        4) echo "¡Hasta luego!"; exit 0 ;;
        *) echo "Opción inválida" ;;
    esac
done
