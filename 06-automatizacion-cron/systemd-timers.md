```markdown
# Systemd Timers

## 📘 Introducción a Systemd Timers

**Systemd timers** son la alternativa moderna a cron. Ofrecen más flexibilidad, mejor integración con systemd y un control más preciso de las tareas.

---

## ⚖️ Cron vs Systemd Timers

| Característica | Cron | Systemd Timers |
|----------------|------|----------------|
| Resolución | Minutos | Microsegundos |
| Dependencias | No | Sí |
| Logs integrados | No | Sí |
| Control de recursos | No | Sí (cgroups) |
| Sintaxis | Simple | Más compleja |
| Historificación | No | Sí (journald) |

---

## 📝 Crear un timer

### Archivos necesarios
1. **Servicio**: `/etc/systemd/system/mi-servicio.service`
2. **Timer**: `/etc/systemd/system/mi-servicio.timer`

### Archivo de servicio
```ini
# /etc/systemd/system/mi-servicio.service
[Unit]
Description=Mi servicio programado

[Service]
Type=oneshot
ExecStart=/usr/local/bin/mi-script.sh
User=usuario
Group=usuario
StandardOutput=journal
StandardError=journal
Archivo de timer
ini
# /etc/systemd/system/mi-servicio.timer
[Unit]
Description=Timer para mi servicio
Requires=mi-servicio.service

[Timer]
# Cuando ejecutar
OnCalendar=*-*-* 02:00:00
OnBootSec=10min
OnUnitActiveSec=1d
RandomizedDelaySec=5min

# Persistencia
Persistent=true

# Unidad a ejecutar
Unit=mi-servicio.service

[Install]
WantedBy=timers.target
🕒 Sintaxis de Calendar
Ejemplos de OnCalendar

# Diario a las 2 AM
OnCalendar=daily
OnCalendar=*-*-* 02:00:00

# Semanal los lunes a las 3 AM
OnCalendar=weekly
OnCalendar=Mon *-*-* 03:00:00

# Mensual el primer día a las 4 AM
OnCalendar=monthly
OnCalendar=*-*-01 04:00:00

# Cada 15 minutos
OnCalendar=*:0/15

# Días laborales a las 9 AM
OnCalendar=Mon..Fri *-*-* 09:00:00

# Cada hora en punto
OnCalendar=*-*-* *:00:00
Eventos especiales

# Al iniciar el sistema
OnBootSec=5min

# Cada vez que el servicio termina
OnUnitActiveSec=1h

# Inmediatamente después del inicio
OnStartupSec=30s
🛠️ Gestión de timers
Comandos básicos

# Listar timers
systemctl list-timers
systemctl list-timers --all

# Ver timer específico
systemctl status mi-servicio.timer

# Habilitar/Deshabilitar
sudo systemctl enable mi-servicio.timer
sudo systemctl disable mi-servicio.timer

# Iniciar/Detener
sudo systemctl start mi-servicio.timer
sudo systemctl stop mi-servicio.timer

# Recargar configuración
sudo systemctl daemon-reload
Ver logs

# Ver logs del timer
sudo journalctl -u mi-servicio.timer

# Ver logs del servicio ejecutado
sudo journalctl -u mi-servicio.service

# Seguir logs
sudo journalctl -u mi-servicio.timer -f
🧪 Ejemplos de timers
Backup diario
ini
# /etc/systemd/system/backup.service
[Unit]
Description=Backup del sistema

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
User=root
Group=root
Nice=10

[Install]
WantedBy=multi-user.target
ini
# /etc/systemd/system/backup.timer
[Unit]
Description=Timer para backup diario

[Timer]
OnCalendar=daily
RandomizedDelaySec=1h
Persistent=true

[Install]
WantedBy=timers.target
Monitoreo cada 5 minutos
ini
# /etc/systemd/system/monitor.service
[Unit]
Description=Monitoreo del sistema

[Service]
Type=oneshot
ExecStart=/usr/local/bin/monitor.sh
User=root
ini
# /etc/systemd/system/monitor.timer
[Unit]
Description=Timer para monitor

[Timer]
OnCalendar=*:0/5
Unit=monitor.service

[Install]
WantedBy=timers.target
🧪 Script de configuración de timers

#!/bin/bash
# setup-systemd-timers.sh

# Crear servicio
sudo cat > /etc/systemd/system/auto-backup.service << 'EOF'
[Unit]
Description=Auto backup del sistema

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
User=root
Group=root
EOF

# Crear timer
sudo cat > /etc/systemd/system/auto-backup.timer << 'EOF'
[Unit]
Description=Timer para auto-backup

[Timer]
OnCalendar=*-*-* 02:00:00
RandomizedDelaySec=30min
Persistent=true

[Install]
WantedBy=timers.target
EOF

# Crear script de backup
sudo cat > /usr/local/bin/backup.sh << 'EOF'
#!/bin/bash
echo "$(date): Iniciando backup" | logger -t backup
tar -czf /backup/backup-$(date +%Y%m%d).tar.gz /home /etc 2>/dev/null
echo "$(date): Backup completado" | logger -t backup
EOF

sudo chmod +x /usr/local/bin/backup.sh

# Recargar y habilitar
sudo systemctl daemon-reload
sudo systemctl enable auto-backup.timer
sudo systemctl start auto-backup.timer

echo "✅ Systemd timer configurado"
echo "📋 Verificar: systemctl list-timers"
