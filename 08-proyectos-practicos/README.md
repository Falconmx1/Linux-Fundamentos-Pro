# Proyecto: Servidor Web LAMP

## 🎯 Objetivo

Configurar un servidor web completo con Apache, MySQL/MariaDB y PHP.

---

## 📋 Requisitos

- Sistema Ubuntu/Debian
- Conexión a internet
- Acceso root/sudo

---

## 🔧 Paso 1: Instalación de Apache

```bash
# Instalar Apache
sudo apt update
sudo apt install apache2

# Verificar estado
sudo systemctl status apache2

# Configurar firewall
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
🗄️ Paso 2: Instalación de MySQL

# Instalar MySQL
sudo apt install mysql-server

# Configuración segura
sudo mysql_secure_installation

# Acceder a MySQL
sudo mysql -u root -p

# Crear base de datos y usuario
CREATE DATABASE mi_app;
CREATE USER 'app_user'@'localhost' IDENTIFIED BY 'password_seguro';
GRANT ALL PRIVILEGES ON mi_app.* TO 'app_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
🐘 Paso 3: Instalación de PHP

# Instalar PHP y módulos
sudo apt install php php-mysql php-gd php-xml php-mbstring

# Verificar
php -v

# Crear archivo de prueba
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
📝 Paso 4: Configurar sitio web

# Crear directorio del sitio
sudo mkdir -p /var/www/mi-sitio

# Crear archivo de configuración
sudo cat > /etc/apache2/sites-available/mi-sitio.conf << 'EOF'
<VirtualHost *:80>
    ServerAdmin admin@ejemplo.com
    ServerName mi-sitio.com
    ServerAlias www.mi-sitio.com
    DocumentRoot /var/www/mi-sitio
    
    <Directory /var/www/mi-sitio>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
EOF

# Habilitar sitio
sudo a2ensite mi-sitio.conf
sudo systemctl reload apache2
🚀 Paso 5: Desplegar aplicación

# Crear aplicación PHP
cat > /var/www/mi-sitio/index.php << 'EOF'
<?php
$servername = "localhost";
$username = "app_user";
$password = "password_seguro";
$dbname = "mi_app";

$conn = new mysqli($servername, $username, $password, $dbname);

if ($conn->connect_error) {
    die("Error de conexión: " . $conn->connect_error);
}

echo "<h1>¡Servidor LAMP funcionando!</h1>";
echo "<p>Conectado a MySQL</p>";

$result = $conn->query("SHOW DATABASES");
echo "<ul>";
while ($row = $result->fetch_assoc()) {
    echo "<li>" . $row['Database'] . "</li>";
}
echo "</ul>";

$conn->close();
?>
EOF

# Verificar
curl localhost/mi-sitio/
📊 Verificación

#!/bin/bash
# verify-server.sh

echo "=== VERIFICACIÓN DEL SERVIDOR ==="

# Apache
if systemctl is-active apache2; then
    echo "✅ Apache: activo"
else
    echo "❌ Apache: inactivo"
fi

# MySQL
if systemctl is-active mysql; then
    echo "✅ MySQL: activo"
else
    echo "❌ MySQL: inactivo"
fi

# PHP
if php -v > /dev/null 2>&1; then
    echo "✅ PHP: instalado"
else
    echo "❌ PHP: no instalado"
fi

# Sitio web
if curl -s localhost/mi-sitio/ | grep -q "Servidor LAMP"; then
    echo "✅ Sitio web: funcionando"
else
    echo "❌ Sitio web: error"
fi
