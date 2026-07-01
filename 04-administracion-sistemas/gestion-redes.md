```markdown
# Gestión de Redes en Linux

## 🌐 Herramientas de red

### Comandos básicos
```bash
# Ver interfaces de red
ip addr show
ip link show
ifconfig (obsoleto, pero disponible)

# Información de red
ip route show
route -n
hostname -I

# Pruebas de conectividad
ping -c 4 google.com
traceroute google.com
mtr google.com   # Mejor que traceroute

# Puertos y conexiones
ss -tulpn         # Puertos escuchando
netstat -tulpn    # Alternativa
nmap localhost    # Escanear puertos
🔧 Configuración de red
Interfaz DHCP (automática)

# Configuración temporal
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up

# DHCP con dhclient
sudo dhclient eth0
Configuración estática - Netplan (Ubuntu)
yaml
# /etc/netplan/01-netcfg.yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1

# Aplicar cambios
sudo netplan apply
Configuración estática - /etc/network/interfaces (Debian)

# /etc/network/interfaces
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 1.1.1.1

# Reiniciar red
sudo systemctl restart networking
🛡️ Configuración de DNS

# Archivo de resolución
cat /etc/resolv.conf

# Cambiar DNS temporalmente
echo "nameserver 8.8.8.8" > /etc/resolv.conf

# Configurar DNS en Netplan (ver arriba)
📊 Resolución de problemas de red

# Verificar conectividad
ping -c 4 8.8.8.8

# Verificar DNS
dig google.com
nslookup google.com

# Verificar ruta
traceroute 8.8.8.8

# Verificar puertos abiertos
ss -tulpn | grep LISTEN

# Reiniciar red
sudo systemctl restart NetworkManager

# Ver logs
journalctl -xe | grep network
🧪 Script de diagnóstico de red

#!/bin/bash
# network-diag.sh - Diagnóstico de red

echo "=== DIAGNÓSTICO DE RED ==="
echo "Fecha: $(date)"
echo

echo "→ Interfaces de red:"
ip addr show | grep -E '^[0-9]|inet '

echo
echo "→ Puerta de enlace:"
ip route show | grep default

echo
echo "→ DNS:"
cat /etc/resolv.conf | grep nameserver

echo
echo "→ Prueba de conectividad:"
ping -c 2 8.8.8.8 &>/dev/null
if [ $? -eq 0 ]; then
    echo "✅ Internet: Conectado"
else
    echo "❌ Internet: Sin conexión"
fi

echo
echo "→ Puertos abiertos:"
ss -tulpn | grep LISTEN | head -10

echo
echo "→ Conexiones activas:"
ss -tun | wc -l
