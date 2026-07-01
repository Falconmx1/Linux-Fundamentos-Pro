```markdown
# Hardening del Sistema Linux

## 🛡️ Hardening General

### Actualizaciones automáticas
```bash
# Instalar unattended-upgrades
sudo apt install unattended-upgrades

# Configurar
sudo dpkg-reconfigure --priority=low unattended-upgrades

# Verificar
sudo systemctl status unattended-upgrades
Kernel hardening

# Configurar sysctl (/etc/sysctl.conf)
# Protección IP forwarding
net.ipv4.ip_forward = 0

# Deshabilitar redirects
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.default.send_redirects = 0

# Protección SYN cookies
net.ipv4.tcp_syncookies = 1

# Deshabilitar source routing
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0

# Deshabilitar ICMP redirects
net.ipv4.conf.all.accept_redirects = 0

# Aplicar
sudo sysctl -p
Usuarios y permisos

# Crear usuario de servicio
sudo useradd -r -s /bin/false servicio

# Asignar permisos mínimos
chmod 644 /etc/passwd
chmod 644 /etc/group
chmod 600 /etc/shadow

# Usar sudoers en lugar de root
sudo visudo
# Añadir:
usuario ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
🛡️ Herramientas de Hardening
Lynis - Auditoría de seguridad
bash
# Instalar
sudo apt install lynis

# Ejecutar auditoría
sudo lynis audit system

# Ver resultados
sudo lynis show report
RKHunter - Detección de rootkits

# Instalar
sudo apt install rkhunter

# Actualizar
sudo rkhunter --update

# Ejecutar
sudo rkhunter --check --skip-keypress
ClamAV - Antivirus

# Instalar
sudo apt install clamav clamav-daemon

# Actualizar
sudo freshclam

# Escanear directorio
sudo clamscan -r /home/usuario

# Escanear todo el sistema (lento)
sudo clamscan -r /
🧪 Script completo de hardening

#!/bin/bash
# hardening-system.sh - Hardening completo del sistema

set -e

echo "=== HARDENING DEL SISTEMA ==="

# 1. Actualizar sistema
echo "Actualizando sistema..."
sudo apt update && sudo apt upgrade -y

# 2. Instalar herramientas de seguridad
echo "Instalando herramientas de seguridad..."
sudo apt install -y fail2ban lynis rkhunter clamav

# 3. Configurar firewall
echo "Configurando firewall..."
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 2222/tcp
sudo ufw enable

# 4. Configurar sysctl
echo "Configurando sysctl..."
sudo cat >> /etc/sysctl.conf << EOF
net.ipv4.tcp_syncookies = 1
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.default.accept_source_route = 0
net.ipv4.conf.all.accept_redirects = 0
EOF
sudo sysctl -p

# 5. Deshabilitar servicios innecesarios
echo "Deshabilitando servicios innecesarios..."
sudo systemctl list-unit-files --type=service --state=enabled | grep -v systemd | while read service; do
    read -p "¿Deshabilitar $service? (y/n): " choice
    if [ "$choice" = "y" ]; then
        sudo systemctl disable "$service"
    fi
done

# 6. Configurar unattended-upgrades
echo "Configurando actualizaciones automáticas..."
sudo dpkg-reconfigure --priority=low unattended-upgrades

# 7. Ejecutar auditoría
echo "Ejecutando auditoría de seguridad..."
sudo lynis audit system

echo "✅ Hardening del sistema completado"
echo "⚠️  Revisa el reporte de lynis para más recomendaciones"
