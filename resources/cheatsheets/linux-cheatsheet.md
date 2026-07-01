```markdown
# Linux Cheatsheet - Hoja de Referencia Rápida

## 📋 Comandos Básicos

### Navegación
```bash
pwd              # Directorio actual
ls               # Listar archivos
ls -la           # Lista detallada
cd ~             # Ir a home
cd ..            # Subir un nivel
cd -             # Directorio anterior
Manipulación de archivos

touch archivo    # Crear archivo
mkdir carpeta    # Crear directorio
cp origen dest   # Copiar
mv origen dest   # Mover/renombrar
rm archivo       # Eliminar
rm -r carpeta    # Eliminar recursivo
Visualización

cat archivo      # Ver contenido
less archivo     # Ver paginado
head -n10        # Primeras 10 líneas
tail -n10        # Últimas 10 líneas
tail -f          # Seguir en tiempo real
🔍 Búsqueda y Filtros
grep
bash
grep "texto" archivo
grep -i "texto" archivo    # Insensible a mayúsculas
grep -r "texto" dir/        # Recursivo
grep -v "excluir" archivo   # Excluir
grep -c "texto" archivo    # Contar
find
bash
find . -name "*.txt"
find . -type f -size +10M
find . -mtime -7            # Últimos 7 días
find . -exec chmod 644 {} \;
sed y awk
bash
sed 's/viejo/nuevo/g' archivo
sed -i 's/viejo/nuevo/g' archivo
awk '{print $1}' archivo
awk -F: '{print $1}' /etc/passwd
🔐 Permisos
Cambiar permisos
bash
chmod 755 archivo
chmod +x script.sh
chmod -R 644 archivos/
chown usuario:grupo archivo
chgrp grupo archivo
Ver permisos
bash
ls -la
stat archivo
📊 Procesos
bash
ps aux                # Todos los procesos
ps aux | grep nginx   # Buscar proceso
top                   # Monitor en tiempo real
htop                  # Monitor mejorado
kill PID              # Terminar proceso
kill -9 PID           # Forzar terminación
killall nombre        # Terminar por nombre
🌐 Red
bash
ip addr show          # Interfaces
ip route show         # Tabla de ruteo
ping -c4 google.com   # Prueba de conectividad
ss -tulpn             # Puertos abiertos
netstat -tulpn        # Alternativa
curl -I ejemplo.com   # Headers HTTP
wget archivo          # Descargar archivo
🗄️ Discos
bash
df -h                 # Espacio en disco
du -sh carpeta/       # Tamaño de carpeta
lsblk                 # Listar discos
fdisk -l              # Información de discos
mount /dev/sdb1 /mnt  # Montar disco
umount /mnt           # Desmontar
📦 Paquetes
APT (Ubuntu/Debian)
bash
apt update
apt upgrade
apt install paquete
apt remove paquete
apt search paquete
apt list --upgradable
YUM/DNF (Fedora/RHEL)
bash
dnf install paquete
dnf remove paquete
dnf search paquete
dnf update
🔄 Systemd

systemctl status servicio
systemctl start servicio
systemctl stop servicio
systemctl restart servicio
systemctl enable servicio
systemctl disable servicio
journalctl -u servicio -f
🐳 Docker
bash
docker ps
docker ps -a
docker images
docker run -it ubuntu bash
docker start/stop container
docker rm container
docker rmi imagen
docker build -t imagen .
docker-compose up -d
docker-compose down
📝 Crontab

crontab -e            # Editar
crontab -l            # Listar
crontab -r            # Eliminar
# Formato:
# * * * * * comando
🧵 Git

git clone url
git add .
git commit -m "mensaje"
git push origin main
git pull
git status
git log --oneline
git branch
git checkout -b nueva-rama
