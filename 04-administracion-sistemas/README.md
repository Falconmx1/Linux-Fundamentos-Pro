```markdown
# 🖥️ Administración de Sistemas Linux

Esta carpeta cubre los fundamentos de la administración de sistemas en Linux, desde la gestión de servicios hasta la seguridad avanzada.

---

## 🎯 Objetivos de aprendizaje

- Gestionar servicios con systemd
- Configurar redes y firewall
- Administrar usuarios y grupos
- Implementar seguridad y permisos
- Monitorear y optimizar el sistema
- Manejar discos y sistemas de archivos

---

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `systemd-servicios.md` | Gestión de servicios con systemd |
| `gestion-redes.md` | Configuración de red, IP, DNS |
| `firewall-iptables.md` | Seguridad con firewall y UFW |
| `usuarios-grupos.md` | Administración avanzada de usuarios |
| `discos-lvm.md` | Gestión de discos y LVM |
| `monitoreo-optimizacion.md` | Herramientas de monitoreo |
| `seguridad-hardening.md` | Mejora de seguridad del sistema |
| `scripts-administracion.md` | Scripts para administradores |

---

## 🚀 Comandos esenciales de administración

```bash
# Servicios
systemctl status nginx
systemctl start nginx
systemctl enable nginx

# Redes
ip addr
ip link set eth0 up
ss -tulpn

# Usuarios
useradd -m usuario
usermod -aG sudo usuario

# Discos
lsblk
fdisk -l
mount /dev/sdb1 /mnt

# Sistema
uptime
dmesg | tail -20
journalctl -xe
