```markdown
# Fundamentos de Cron

## 📘 ¿Qué es Cron?

**Cron** es un sistema de programación de tareas en Linux que ejecuta comandos o scripts automáticamente en fechas y horas específicas.

---

## 📝 Sintaxis de Crontab

### Formato básico
comando_a_ejecutar
│ │ │ │ │
│ │ │ │ └── Día de la semana (0-7, 0=domingo)
│ │ │ └──── Mes (1-12)
│ │ └────── Día del mes (1-31)
│ └──────── Hora (0-23)
└────────── Minuto (0-59)

text

### Ejemplos de intervalos
| Sintaxis | Significado |
|----------|-------------|
| `*` | Cada unidad (ej: cada minuto) |
| `*/5` | Cada 5 unidades |
| `1-5` | Rango (1 al 5) |
| `1,3,5` | Lista (1, 3 y 5) |
| `0 2 * * *` | A las 2:00 AM todos los días |
| `*/15 * * * *` | Cada 15 minutos |
| `0 0 * * 0` | Todos los domingos a las 12 AM |
| `0 9-17 * * 1-5` | Cada hora de 9 AM a 5 PM, de lunes a viernes |

---

## 🛠️ Comandos de Crontab

### Gestión básica
```bash
# Editar crontab
crontab -e

# Listar trabajos
crontab -l

# Eliminar todos los trabajos
crontab -r

# Editar como otro usuario
sudo crontab -u otro_usuario -e
Archivos de cron

# Crontab del sistema
/etc/crontab

# Directorio de cron diario
/etc/cron.d/

# Scripts por periodo
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
📊 Variables de entorno en cron

# Definir variables en crontab
SHELL=/bin/bash
PATH=/usr/local/bin:/usr/bin:/bin
MAILTO=usuario@ejemplo.com

# Usar variables
0 2 * * * $HOME/scripts/backup.sh
🧪 Ejemplos prácticos

# 1. Backup de base de datos
# /etc/cron.d/db-backup
0 1 * * * root /usr/local/bin/backup-mysql.sh

# 2. Rotación de logs
# /etc/cron.daily/logrotate
/usr/sbin/logrotate /etc/logrotate.conf

# 3. Script personalizado
# Crontab del usuario
# Actualizar sistema cada martes
0 3 * * 2 sudo apt update && sudo apt upgrade -y

# 4. Enviar reporte por email
0 8 * * 1 /usr/local/bin/reporte.sh | mail -s "Reporte semanal" admin@ejemplo.com
🧪 Script de configuración de cron

#!/bin/bash
# setup-cron.sh - Configuración inicial de cron

echo "=== CONFIGURACIÓN DE CRON ==="

# Crear directorios de scripts
mkdir -p ~/cron-scripts
mkdir -p ~/cron-logs

# Backup script
cat > ~/cron-scripts/backup.sh << 'EOF'
#!/bin/bash
echo "$(date): Ejecutando backup..." >> ~/cron-logs/backup.log
# Añadir comandos de backup aquí
echo "$(date): Backup completado" >> ~/cron-logs/backup.log
EOF

chmod +x ~/cron-scripts/backup.sh

# Añadir a crontab
(crontab -l 2>/dev/null; echo "0 2 * * * ~/cron-scripts/backup.sh") | crontab -

echo "✅ Script de backup configurado"
echo "📋 Verifica con: crontab -l"
