```markdown
# Firewall: Iptables y UFW

## 🛡️ Iptables - El firewall de Linux

### Estructura básica
```bash
# Tablas principales
filter  # Tabla por defecto (permitir/denegar)
nat     # Traducción de direcciones
mangle  # Modificación de paquetes

# Cadenas (chains)
INPUT   # Paquetes entrantes
OUTPUT  # Paquetes salientes
FORWARD # Paquetes enrutados
Comandos básicos de iptables
bash
# Ver reglas
sudo iptables -L -n -v
sudo iptables -L -n -v --line-numbers

# Políticas por defecto
sudo iptables -P INPUT DROP
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD DROP

# Reglas básicas
# Permitir SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Permitir HTTP/HTTPS
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# Permitir conexiones establecidas
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Bloquear IP específica
sudo iptables -A INPUT -s 192.168.1.100 -j DROP

# Limitar conexiones SSH
sudo iptables -A INPUT -p tcp --dport 22 -m connlimit --connlimit-above 5 -j DROP
Reglas avanzadas

# Protección contra ataques DoS
sudo iptables -A INPUT -p tcp --dport 80 -m limit --limit 25/minute --limit-burst 100 -j ACCEPT

# Bloquear puertos comunes
sudo iptables -A INPUT -p tcp --dport 135:139 -j DROP
sudo iptables -A INPUT -p udp --dport 135:139 -j DROP

# Redirección de puertos (NAT)
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80

# Fragmentos de IP (prevención)
sudo iptables -A INPUT -f -j DROP
🎨 UFW - Uncomplicated Firewall
Comandos básicos

# Instalar
sudo apt install ufw

# Habilitar/Deshabilitar
sudo ufw enable
sudo ufw disable

# Ver estado
sudo ufw status verbose
sudo ufw status numbered

# Reglas básicas
sudo ufw allow 22/tcp
sudo ufw allow 80,443/tcp
sudo ufw allow from 192.168.1.0/24 to any port 22
sudo ufw deny from 10.0.0.0/8

# Reglas avanzadas
sudo ufw allow proto tcp from 192.168.1.0/24 to any port 22
sudo ufw deny proto tcp to any port 3306

# Eliminar regla
sudo ufw delete allow 80
Configuración por aplicación

# Ver aplicaciones disponibles
sudo ufw app list

# Permitir por aplicación
sudo ufw allow "OpenSSH"
sudo ufw allow "Nginx Full"
sudo ufw allow "Apache Full"
📊 Monitoreo de firewall

# Ver logs de iptables
sudo journalctl -xe | grep iptables
sudo tail -f /var/log/kern.log | grep DPT

# Ver conexiones actuales
sudo conntrack -L
sudo netstat -tun | grep ESTABLISHED

# Script de monitoreo
#!/bin/bash
# firewall-monitor.sh

echo "=== ESTADO DEL FIREWALL ==="
echo "Reglas de INPUT:"
sudo iptables -L INPUT -n -v | head -20

echo
echo "Reglas de FORWARD:"
sudo iptables -L FORWARD -n -v | head -10

echo
echo "Conexiones activas:"
sudo conntrack -L | wc -l
🧪 Script de firewall completo

#!/bin/bash
# firewall-setup.sh - Configuración completa de firewall

# Resetear iptables
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t mangle -F

# Políticas por defecto
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Permitir loopback
sudo iptables -A INPUT -i lo -j ACCEPT

# Permitir conexiones establecidas
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT

# Permitir SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Permitir HTTP/HTTPS
sudo iptables -A INPUT -p tcp -m multiport --dports 80,443 -j ACCEPT

# Protección SYN flood
sudo iptables -A INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT

# Protección port scanning
sudo iptables -A INPUT -m recent --name portscan --rcheck --seconds 60 -j DROP
sudo iptables -A INPUT -m recent --name portscan --set -j DROP

# Logging
sudo iptables -A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7

# Guardar configuración
sudo iptables-save > /etc/iptables/rules.v4
sudo ip6tables-save > /etc/iptables/rules.v6

echo "✅ Firewall configurado correctamente"
