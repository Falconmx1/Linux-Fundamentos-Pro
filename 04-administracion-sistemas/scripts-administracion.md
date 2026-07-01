```markdown
# Scripts de Administración

## 🛠️ Scripts útiles para administradores

### Script 1: Monitor de recursos
```bash
#!/bin/bash
# sys-monitor.sh - Monitoreo completo del sistema

# Colores
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

# Almacenar datos
MEM_TOTAL=$(free -m | grep Mem | awk '{print $2}')
MEM_USED=$(free -m | grep Mem | awk '{print $3}')
MEM_PERCENT=$((MEM_USED * 100 / MEM_TOTAL))

CPU_LOAD=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)
DISK_USED=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

# Mostrar
echo "=== MONITOR DEL SISTEMA ==="
echo "Fecha: $(date)"
echo "Uptime: $(uptime -p)"
echo

# CPU
echo "CPU: $CPU_LOAD%"
if [ $CPU_LOAD -gt 80 ]; then
    echo -e "${RED}⚠️  ALTA CARGA DE CPU${NC}"
fi

# Memoria
echo "Memoria: $MEM_USED MB / $MEM_TOTAL MB ($MEM_PERCENT%)"
if [ $MEM_PERCENT -gt 85 ]; then
    echo -e "${RED}⚠️  MEMORIA BAJA${NC}"
fi

# Disco
echo "Disco: $DISK_USED%"
if [ $DISK_USED -gt 85 ]; then
    echo -e "${RED}⚠️  ESPACIO DE DISCO BAJO${NC}"
fi

echo "=========================="
Script 2: Backup de configuración

#!/bin/bash
# config-backup.sh - Backup de configuraciones del sistema

BACKUP_DIR="/backup/configs"
DATE=$(date +%Y%m%d)

# Directorios a respaldar
CONFIGS=(
    "/etc"
    "/home"
    "/var/www"
    "/var/lib/mysql"
)

mkdir -p "$BACKUP_DIR"

echo "=== BACKUP DE CONFIGURACIONES ==="
for dir in "${CONFIGS[@]}"; do
    if [ -d "$dir" ]; then
        name=$(basename "$dir")
        output="${BACKUP_DIR}/${name}_${DATE}.tar.gz"
        echo "Respaldando: $dir → $output"
        tar -czf "$output" "$dir" 2>/dev/null
    else
        echo "⚠️  $dir no existe"
    fi
done

echo "✅ Backup completado en: $BACKUP_DIR"
ls -lh "$BACKUP_DIR"
Script 3: Limpieza de sistema

#!/bin/bash
# system-clean.sh - Limpieza del sistema

echo "=== LIMPIEZA DEL SISTEMA ==="

# Limpiar cache de apt
if command -v apt &>/dev/null; then
    echo "Limpiando cache de apt..."
    sudo apt clean
    sudo apt autoclean
    sudo apt autoremove -y
fi

# Limpiar logs viejos
echo "Limpiando logs viejos..."
sudo journalctl --vacuum-time=7d

# Limpiar cache de usuarios
echo "Limpiando cache de usuarios..."
rm -rf ~/.cache/* 2>/dev/null

# Mostrar espacio liberado
echo "Espacio en disco después de limpieza:"
df -h /
Script 4: Escáner de puertos

#!/bin/bash
# port-scanner.sh - Escáner básico de puertos

if [ -z "$1" ]; then
    echo "Uso: $0 <ip>"
    exit 1
fi

IP="$1"
echo "Escaneando: $IP"
echo "========================"

# Puertos comunes
for port in 22 80 443 21 25 3306 5432 8080; do
    timeout 1 bash -c "echo >/dev/tcp/$IP/$port" 2>/dev/null
    if [ $? -eq 0 ]; then
        echo "✅ Puerto $port abierto"
    else
        echo "❌ Puerto $port cerrado"
    fi
done
Script 5: Reporte del sistema

#!/bin/bash
# system-report.sh - Genera reporte completo

REPORT_FILE="system_report_$(date +%Y%m%d).txt"

{
    echo "=== REPORTE DEL SISTEMA ==="
    echo "Fecha: $(date)"
    echo "Hostname: $(hostname)"
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
    ip addr show | grep -E '^[0-9]|inet '
    echo
    echo "--- SERVICIOS ACTIVOS ---"
    systemctl list-units --type=service --state=running | head -10
    echo
    echo "--- ÚLTIMOS LOGS ---"
    journalctl -xe -n 10
} > "$REPORT_FILE"

echo "✅ Reporte generado: $REPORT_FILE"
📋 Scripts de automatización
Cron jobs para administradores

# /etc/crontab o crontab -e

# Backup diario a las 2 AM
0 2 * * * /usr/local/bin/backup.sh

# Limpieza semanal los domingos a las 3 AM
0 3 * * 0 /usr/local/bin/cleanup.sh

# Reporte de sistema cada hora
0 * * * * /usr/local/bin/system-monitor.sh

# Monitoreo de servicios cada 5 minutos
*/5 * * * * /usr/local/bin/service-check.sh
