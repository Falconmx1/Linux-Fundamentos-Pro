```markdown
# Scripts de Automatización

## 🛠️ Scripts útiles para automatización

### Script 1: Backup completo del sistema
```bash
#!/bin/bash
# backup-full.sh - Backup completo del sistema

BACKUP_DIR="/backup"
DATE=$(date +%Y%m%d_%H%M%S)
LOG_FILE="/var/log/backup-${DATE}.log"

# Directorios a respaldar
DIRS=(
    "/home"
    "/etc"
    "/var/www"
    "/var/lib/mysql"
)

# Archivos especiales
FILES=(
    "/etc/ssh/sshd_config"
    "/etc/sudoers"
    "/etc/fstab"
)

echo "=== BACKUP COMPLETO $(date) ===" | tee -a "$LOG_FILE"

# Crear directorio de backup
mkdir -p "$BACKUP_DIR/${DATE}"

# Backup de directorios
for dir in "${DIRS[@]}"; do
    if [ -d "$dir" ]; then
        name=$(basename "$dir")
        echo "Backup: $dir" | tee -a "$LOG_FILE"
        tar -czf "$BACKUP_DIR/${DATE}/${name}.tar.gz" -C / "$(echo $dir | sed 's|^/||')" 2>/dev/null
    fi
done

# Backup de archivos individuales
mkdir -p "$BACKUP_DIR/${DATE}/files"
for file in "${FILES[@]}"; do
    if [ -f "$file" ]; then
        cp "$file" "$BACKUP_DIR/${DATE}/files/"
        echo "Copiado: $file" | tee -a "$LOG_FILE"
    fi
done

# Backup de base de datos MySQL
if command -v mysqldump &>/dev/null; then
    echo "Backup de MySQL..." | tee -a "$LOG_FILE"
    mysqldump --all-databases > "$BACKUP_DIR/${DATE}/mysql-dump.sql" 2>/dev/null
fi

# Crear sumas de verificación
echo "Generando checksums..." | tee -a "$LOG_FILE"
cd "$BACKUP_DIR/${DATE}"
sha256sum * > checksums.sha256

# Limpiar backups viejos (30 días)
find "$BACKUP_DIR" -maxdepth 1 -type d -mtime +30 -exec rm -rf {} \;

echo "✅ Backup completado en: $BACKUP_DIR/${DATE}" | tee -a "$LOG_FILE"
Script 2: Limpieza automática

#!/bin/bash
# cleanup-auto.sh - Limpieza automática del sistema

LOG_FILE="/var/log/cleanup.log"

echo "$(date): Iniciando limpieza..." >> "$LOG_FILE"

# Limpiar apt cache
if command -v apt &>/dev/null; then
    echo "Limpiando apt cache..." >> "$LOG_FILE"
    apt clean
    apt autoclean
    apt autoremove -y
fi

# Limpiar journald
echo "Limpiando logs viejos..." >> "$LOG_FILE"
journalctl --vacuum-size=100M
journalctl --vacuum-time=7d

# Limpiar /tmp (archivos viejos de +7 días)
echo "Limpiando /tmp..." >> "$LOG_FILE"
find /tmp -type f -atime +7 -delete
find /tmp -type d -empty -delete

# Limpiar cache de usuarios
for user in /home/*; do
    if [ -d "$user/.cache" ]; then
        echo "Limpiando cache de $user..." >> "$LOG_FILE"
        find "$user/.cache" -type f -atime +7 -delete
    fi
done

# Reporte de espacio liberado
echo "Espacio liberado:" >> "$LOG_FILE"
df -h / >> "$LOG_FILE"

echo "$(date): Limpieza completada" >> "$LOG_FILE"
Script 3: Monitoreo y alertas

#!/bin/bash
# monitor-alerts.sh - Monitoreo con alertas

EMAIL="admin@ejemplo.com"
THRESHOLD_CPU=80
THRESHOLD_MEM=85
THRESHOLD_DISK=80

# Función para enviar alerta
send_alert() {
    local subject="$1"
    local body="$2"
    echo "$body" | mail -s "$subject" "$EMAIL"
}

# CPU
CPU_LOAD=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)
if [ "$CPU_LOAD" -gt "$THRESHOLD_CPU" ]; then
    send_alert "⚠️ Alerta: CPU alta" "CPU: ${CPU_LOAD}% (Límite: ${THRESHOLD_CPU}%)"
fi

# Memoria
MEM_TOTAL=$(free -m | grep Mem | awk '{print $2}')
MEM_USED=$(free -m | grep Mem | awk '{print $3}')
MEM_PERCENT=$((MEM_USED * 100 / MEM_TOTAL))
if [ "$MEM_PERCENT" -gt "$THRESHOLD_MEM" ]; then
    send_alert "⚠️ Alerta: Memoria baja" "Memoria: ${MEM_PERCENT}% (Límite: ${THRESHOLD_MEM}%)"
fi

# Disco
DISK_USED=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ "$DISK_USED" -gt "$THRESHOLD_DISK" ]; then
    send_alert "⚠️ Alerta: Espacio en disco bajo" "Disco: ${DISK_USED}% (Límite: ${THRESHOLD_DISK}%)"
fi

# Servicios críticos
services=("nginx" "mysql" "ssh")
for service in "${services[@]}"; do
    if systemctl is-active "$service" &>/dev/null; then
        echo "✅ $service: activo"
    else
        send_alert "🚨 Servicio caído: $service" "El servicio $service no está en ejecución"
    fi
done
Script 4: Rotación de logs
bash
#!/bin/bash
# log-rotation.sh - Rotación automática de logs

LOG_DIR="/var/log/mi-app"
MAX_SIZE=100M
MAX_FILES=10

mkdir -p "$LOG_DIR/archive"

# Rotar logs
for log in "$LOG_DIR"/*.log; do
    if [ -f "$log" ]; then
        size=$(du -b "$log" | awk '{print $1}')
        size_mb=$((size / 1024 / 1024))
        
        if [ "$size_mb" -gt "${MAX_SIZE%M}" ]; then
            base=$(basename "$log" .log)
            date=$(date +%Y%m%d_%H%M%S)
            mv "$log" "$LOG_DIR/archive/${base}_${date}.log"
            gzip "$LOG_DIR/archive/${base}_${date}.log"
            
            # Crear nuevo archivo de log
            touch "$log"
            chmod 644 "$log"
            
            echo "$(date): Rotado $log (tamaño: ${size_mb}MB)" >> "$LOG_DIR/rotation.log"
        fi
    fi
done

# Limpiar logs viejos
find "$LOG_DIR/archive" -name "*.gz" -mtime +30 -delete
Script 5: Reporte diario del sistema
bash
#!/bin/bash
# daily-report.sh - Reporte diario del sistema

REPORT_FILE="/var/reports/system-report-$(date +%Y%m%d).txt"

{
    echo "=== REPORTE DEL SISTEMA ==="
    echo "Fecha: $(date)"
    echo "Hostname: $(hostname)"
    echo "Uptime: $(uptime -p)"
    echo "Usuario: $(whoami)"
    echo
    
    echo "--- SISTEMA ---"
    uname -a
    echo
    
    echo "--- CPU ---"
    lscpu | grep -E "Model name|CPU\(s\)|MHz"
    echo
    
    echo "--- MEMORIA ---"
    free -h
    echo
    
    echo "--- DISCO ---"
    df -h
    echo
    
    echo "--- RED ---"
    ip addr show | grep -E '^[0-9]|inet ' | grep -v "127.0.0.1"
    echo
    
    echo "--- SERVICIOS ---"
    systemctl list-units --type=service --state=running | head -10
    echo
    
    echo "--- PROCESOS MÁS PESADOS ---"
    ps aux --sort=-%mem | head -10
    echo
    
    echo "--- ÚLTIMOS LOGS DE ERROR ---"
    journalctl -p 3 -n 5
    echo
    
    echo "--- ACTUALIZACIONES PENDIENTES ---"
    if command -v apt &>/dev/null; then
        apt list --upgradable 2>/dev/null | head -10
    fi
} > "$REPORT_FILE"

# Enviar por email
mail -s "Reporte diario $(date +%Y-%m-%d)" admin@ejemplo.com < "$REPORT_FILE"

echo "✅ Reporte generado: $REPORT_FILE"
📋 Configuración de crontab para scripts
bash
# /etc/crontab o crontab -e

# Backup completo a las 2 AM
0 2 * * * root /usr/local/bin/backup-full.sh

# Limpieza automática a las 3 AM
0 3 * * * root /usr/local/bin/cleanup-auto.sh

# Monitoreo cada 5 minutos
*/5 * * * * root /usr/local/bin/monitor-alerts.sh

# Rotación de logs a las 4 AM
0 4 * * * root /usr/local/bin/log-rotation.sh

# Reporte diario a las 8 AM
0 8 * * * root /usr/local/bin/daily-report.sh
