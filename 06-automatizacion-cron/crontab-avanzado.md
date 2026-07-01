```markdown
# Crontab Avanzado

## 🚀 Sintaxis avanzada

### Cadenas especiales
| Cadena | Equivalente | Significado |
|--------|-------------|-------------|
| `@reboot` | - | Al iniciar el sistema |
| `@yearly` | `0 0 1 1 *` | Cada año |
| `@monthly` | `0 0 1 * *` | Cada mes |
| `@weekly` | `0 0 * * 0` | Cada semana |
| `@daily` | `0 0 * * *` | Cada día |
| `@hourly` | `0 * * * *` | Cada hora |

### Ejemplos con cadenas especiales
```bash
# Script al iniciar el sistema
@reboot /usr/local/bin/startup.sh

# Backup semanal
@weekly /usr/local/bin/backup.sh

# Reporte mensual
@monthly /usr/local/bin/reporte-mensual.sh
📊 Manejo de logs
Redirección de salida

# Redirigir toda salida a log
0 2 * * * /usr/local/bin/backup.sh > /var/log/backup.log 2>&1

# Solo errores
0 2 * * * /usr/local/bin/backup.sh 2> /var/log/backup-error.log

# Añadir a log existente
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1

# Descartar salida
0 2 * * * /usr/local/bin/backup.sh > /dev/null 2>&1
Logging avanzado

# Log con timestamp
0 2 * * * echo "$(date): Iniciando backup" >> /var/log/backup.log && /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1 && echo "$(date): Backup completado" >> /var/log/backup.log

# Usar logger
0 2 * * * /usr/local/bin/backup.sh | logger -t backup-script
🧪 Ejemplos avanzados
Tareas condicionales

# Ejecutar solo si el sistema tiene menos de 80% de uso
0 * * * * [ $(df -h / | awk 'NR==2 {print $5}' | sed 's/%//') -lt 80 ] && /usr/local/bin/limpieza.sh

# Verificar conectividad antes de ejecutar
0 4 * * * ping -c 1 google.com > /dev/null && /usr/local/bin/update.sh

# Ejecutar en días pares
0 0 2-30/2 * * /usr/local/bin/tarea-par.sh
Paralelismo

# Ejecutar varias tareas en paralelo
0 2 * * * /usr/local/bin/tarea1.sh &
0 2 * * * /usr/local/bin/tarea2.sh &
0 2 * * * /usr/local/bin/tarea3.sh &
Lock file (evitar ejecución concurrente)

#!/bin/bash
# script-con-lock.sh

LOCK_FILE="/tmp/mi-script.lock"

if [ -f "$LOCK_FILE" ]; then
    echo "El script ya está en ejecución"
    exit 1
fi

touch "$LOCK_FILE"
trap "rm -f $LOCK_FILE" EXIT

# Resto del script
echo "Ejecutando..."
sleep 60
📊 Monitorización de cron

# Ver logs de cron
sudo journalctl -u cron -f
sudo tail -f /var/log/syslog | grep CRON

# Script de monitoreo
#!/bin/bash
# cron-monitor.sh

echo "=== MONITOR DE CRON ==="
echo "Trabajos programados para $(whoami):"
crontab -l

echo
echo "Últimas ejecuciones:"
sudo grep CRON /var/log/syslog | tail -10

echo
echo "Scripts en directorios de cron:"
ls -la /etc/cron.d/
ls -la /etc/cron.daily/
ls -la /etc/cron.hourly/
🧪 Script de crontab avanzado

#!/bin/bash
# crontab-advanced.sh - Configuración avanzada de crontab

cat > ~/crontab-avanzado << 'EOF'
# Variables de entorno
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
MAILTO=admin@ejemplo.com

# Rotación de logs
@daily /usr/sbin/logrotate /etc/logrotate.conf

# Backup con lock
0 2 * * * flock -n /tmp/backup.lock /usr/local/bin/backup.sh

# Limpieza con verificación
0 3 * * * [ $(df -h / | awk 'NR==2 {print $5}' | sed 's/%//') -gt 80 ] && /usr/local/bin/limpieza.sh

# Monitoreo y alerta
*/5 * * * * /usr/local/bin/monitor.sh && echo "Sistema OK" || echo "Sistema ALERTA" | mail -s "Alerta sistema" admin@ejemplo.com

# Actualización automática (solo seguridad)
0 4 * * 2 sudo apt update && sudo apt install -y unattended-upgrades && sudo unattended-upgrade -d

# Backup SQL con nombre de día
0 1 * * * mysqldump --all-databases > /backup/db-$(date +\%A).sql

# Limpieza de logs viejos (30 días)
0 5 * * 0 find /var/log -name "*.log" -mtime +30 -delete
EOF

crontab ~/crontab-avanzado
echo "✅ Crontab avanzado configurado"
crontab -l
