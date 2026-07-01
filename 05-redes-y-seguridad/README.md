# 🔒 Redes y Seguridad en Linux

Esta carpeta cubre los fundamentos de redes y seguridad en Linux, desde la configuración avanzada de red hasta la implementación de firewalls, SSH y hardening del sistema.

---

## 🎯 Objetivos de aprendizaje

- Configurar redes avanzadas y routing
- Implementar firewall con iptables/UFW
- Configurar y asegurar SSH
- Gestionar certificados SSL/TLS
- Implementar VPN y túneles
- Realizar hardening del sistema
- Monitorear seguridad con herramientas especializadas

---

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `redes-avanzadas.md` | Routing, bridging, VLANs |
| `firewall-iptables.md` | Configuración avanzada de firewall |
| `ssh-seguridad.md` | Configuración y hardening de SSH |
| `ssl-certificados.md` | SSL/TLS y certificados |
| `vpn-tuneles.md` | VPN, túneles SSH, WireGuard |
| `hardening-sistema.md` | Mejora de seguridad del sistema |
| `monitoreo-seguridad.md` | Herramientas de monitoreo |
| `scripts-seguridad.md` | Scripts de seguridad |

---

## 🚀 Comandos esenciales de seguridad

```bash
# Firewall
sudo ufw status
sudo ufw allow 22
sudo iptables -L -n -v

# SSH
ssh-keygen -t ed25519
ssh-copy-id usuario@servidor
sudo systemctl status ssh

# Certificados
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365

# Monitoreo
sudo tcpdump -i eth0
sudo netstat -tulpn
sudo ss -tulpn

# Seguridad
sudo fail2ban-client status
sudo rkhunter --check
sudo lynis audit system
