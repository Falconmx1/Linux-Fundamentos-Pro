```markdown
# SSH: Configuración y Seguridad

## 🔐 Configuración básica de SSH

### Instalación
```bash
# Instalar servidor SSH
sudo apt install openssh-server

# Ver estado
sudo systemctl status ssh

# Configuración principal
sudo nano /etc/ssh/sshd_config
Configuración segura (/etc/ssh/sshd_config)

# Cambiar puerto (recomendado)
Port 2222

# Deshabilitar root login
PermitRootLogin no

# Usar solo protocolo 2
Protocol 2

# Deshabilitar autenticación por contraseña (si usas llaves)
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no

# Autenticación por llaves
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys

# Limitar intentos
MaxAuthTries 3
MaxSessions 5

# Deshabilitar forwarding
AllowTcpForwarding no
X11Forwarding no

# Tiempo de inactividad
ClientAliveInterval 300
ClientAliveCountMax 2

# Logging
LogLevel VERBOSE

# Usuarios permitidos
AllowUsers usuario1 usuario2

# Grupos permitidos
AllowGroups sudo ssh-users
Reiniciar SSH después de cambios

sudo systemctl restart ssh
sudo systemctl status ssh
🔑 Gestión de llaves SSH
Crear llaves

# Ed25519 (recomendado)
ssh-keygen -t ed25519 -C "comentario@ejemplo.com"

# RSA (compatibilidad)
ssh-keygen -t rsa -b 4096 -C "comentario@ejemplo.com"

# Con passphrase
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N "mi_passphrase"
Copiar llave al servidor

# Método automático
ssh-copy-id usuario@servidor

# Método manual
cat ~/.ssh/id_ed25519.pub | ssh usuario@servidor "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
Configuración de cliente (~/.ssh/config)

# Configuración para servidor específico
Host servidor1
    HostName 192.168.1.100
    Port 2222
    User usuario1
    IdentityFile ~/.ssh/id_ed25519

Host servidor2
    HostName ejemplo.com
    Port 2222
    User admin
    IdentityFile ~/.ssh/id_rsa
    ProxyCommand ssh usuario@bastion -W %h:%p
🛡️ Hardening de SSH
Fail2ban - Protección contra ataques de fuerza bruta

# Instalar
sudo apt install fail2ban

# Configurar
sudo nano /etc/fail2ban/jail.local

[sshd]
enabled = true
port = 2222
logpath = %(sshd_log)s
backend = %(sshd_backend)s
maxretry = 3
bantime = 3600
findtime = 600

# Reiniciar
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd
Port Knocking
bash
# Instalar knockd
sudo apt install knockd

# Configurar /etc/knockd.conf
[options]
    LogFile = /var/log/knockd.log

[openSSH]
    sequence = 7000,8000,9000
    seq_timeout = 5
    command = /sbin/iptables -A INPUT -s %IP% -p tcp --dport 2222 -j ACCEPT
    tcpflags = syn

[closeSSH]
    sequence = 9000,8000,7000
    seq_timeout = 5
    command = /sbin/iptables -D INPUT -s %IP% -p tcp --dport 2222 -j ACCEPT
    tcpflags = syn

# Iniciar servicio
sudo systemctl start knockd
sudo systemctl enable knockd

# Knocking (cliente)
knock servidor 7000 8000 9000
2FA (Two-Factor Authentication)

# Instalar Google Authenticator
sudo apt install libpam-google-authenticator

# Configurar PAM
sudo nano /etc/pam.d/sshd
# Añadir al final:
auth required pam_google_authenticator.so

# Configurar SSH
sudo nano /etc/ssh/sshd_config
# Añadir:
ChallengeResponseAuthentication yes
AuthenticationMethods publickey,keyboard-interactive

# Configurar usuario
google-authenticator
📊 Monitoreo de SSH

# Ver conexiones activas
sudo ss -tulpn | grep :22
sudo netstat -tun | grep :22

# Ver logs
sudo journalctl -u ssh -f
sudo tail -f /var/log/auth.log | grep sshd

# Script de monitoreo
#!/bin/bash
# ssh-monitor.sh

echo "=== MONITOR DE SSH ==="
echo "Conexiones activas:"
ss -tun | grep :22 | wc -l

echo
echo "Últimos intentos fallidos:"
sudo grep "Failed password" /var/log/auth.log | tail -5

echo
echo "Usuarios bloqueados por fail2ban:"
sudo fail2ban-client status sshd | grep "Currently banned"
🧪 Script de hardening SSH

#!/bin/bash
# ssh-hardening.sh - Hardening automático de SSH

SSHD_CONFIG="/etc/ssh/sshd_config"
BACKUP_FILE="${SSHD_CONFIG}.backup.$(date +%Y%m%d)"

# Backup
sudo cp "$SSHD_CONFIG" "$BACKUP_FILE"
echo "✅ Backup creado: $BACKUP_FILE"

# Aplicar hardening
echo "Aplicando hardening a SSH..."

sudo sed -i 's/^#Port 22/Port 2222/' "$SSHD_CONFIG"
sudo sed -i 's/^#PermitRootLogin.*/PermitRootLogin no/' "$SSHD_CONFIG"
sudo sed -i 's/^#MaxAuthTries.*/MaxAuthTries 3/' "$SSHD_CONFIG"
sudo sed -i 's/^#ClientAliveInterval.*/ClientAliveInterval 300/' "$SSHD_CONFIG"
sudo sed -i 's/^#ClientAliveCountMax.*/ClientAliveCountMax 2/' "$SSHD_CONFIG"

# Verificar si PasswordAuthentication está habilitado
if grep -q "^PasswordAuthentication yes" "$SSHD_CONFIG"; then
    echo "⚠️  PasswordAuthentication está habilitado. Considera usar solo llaves."
fi

# Instalar fail2ban
if ! command -v fail2ban-client &>/dev/null; then
    echo "Instalando fail2ban..."
    sudo apt install -y fail2ban
    sudo systemctl enable fail2ban
    sudo systemctl start fail2ban
fi

# Reiniciar SSH
sudo systemctl restart ssh

echo "✅ Hardening SSH completado"
echo "📝 Puerto configurado: 2222"
echo "🔑 Usa ssh-copy-id para copiar tus llaves"
