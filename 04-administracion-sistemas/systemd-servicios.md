```markdown
# Systemd y Gestión de Servicios

## 📘 Introducción a systemd

**systemd** es el sistema de inicio y gestión de servicios estándar en la mayoría de las distribuciones Linux modernas (Ubuntu, Debian, Fedora, CentOS, etc.).

---

## 🛠️ Comandos básicos de systemd

### Gestión de servicios
```bash
# Ver estado del servicio
systemctl status nginx

# Iniciar/Detener/Reiniciar
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx

# Habilitar/Deshabilitar (inicio automático)
sudo systemctl enable nginx
sudo systemctl disable nginx

# Recargar configuración sin reiniciar
sudo systemctl reload nginx

# Verificar si está habilitado
systemctl is-enabled nginx
Listar servicios

# Todos los servicios
systemctl list-units --type=service

# Servicios activos
systemctl list-units --type=service --state=running

# Servicios fallidos
systemctl list-units --type=service --state=failed

# Servicios habilitados
systemctl list-unit-files --type=service --state=enabled

📝 Crear un servicio personalizado
Archivo de servicio: /etc/systemd/system/mi-servicio.service
ini
[Unit]
Description=Mi servicio personalizado
After=network.target

[Service]
Type=simple
User=usuario
WorkingDirectory=/home/usuario/mi-app
ExecStart=/usr/bin/python3 /home/usuario/mi-app/app.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
Habilitar y ejecutar

# Recargar systemd
sudo systemctl daemon-reload

# Iniciar el servicio
sudo systemctl start mi-servicio

# Habilitar en el arranque
sudo systemctl enable mi-servicio

# Ver logs
sudo journalctl -u mi-servicio -f
📋 Tipos de servicios
Tipo	Descripción
simple	El proceso principal se ejecuta en primer plano
forking	El proceso principal hace fork y sale
oneshot	Se ejecuta una vez y termina
idle	Se ejecuta cuando el sistema está idle
notify	Notifica a systemd cuando está listo
🧪 Ejemplo: Script como servicio

#!/bin/bash
# /usr/local/bin/mi-monitor.sh - Script de monitoreo

while true; do
    echo "$(date) - Monitoreando..." >> /var/log/monitor.log
    sleep 60
done
Archivo de servicio
ini
[Unit]
Description=Monitor del sistema
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/mi-monitor.sh
Restart=always
StandardOutput=append:/var/log/monitor-service.log
StandardError=append:/var/log/monitor-service.err

[Install]
WantedBy=multi-user.target
