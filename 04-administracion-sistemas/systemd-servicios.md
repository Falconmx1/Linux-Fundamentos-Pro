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
