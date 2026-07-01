```markdown
# Ejercicios Prácticos - Comandos Esenciales

## 🎯 Ejercicio 1: Organización de archivos

```bash
# 1. Crea la siguiente estructura
mkdir -p ~/ejercicios-linux/{descargas,documentos,imagenes,scripts}

# 2. Crea archivos de prueba
touch ~/ejercicios-linux/descargas/archivo1.txt
touch ~/ejercicios-linux/descargas/archivo2.pdf
touch ~/ejercicios-linux/descargas/imagen.png

# 3. Mueve los archivos a sus carpetas correctas
mv ~/ejercicios-linux/descargas/*.txt ~/ejercicios-linux/documentos/
mv ~/ejercicios-linux/descargas/*.pdf ~/ejercicios-linux/documentos/
mv ~/ejercicios-linux/descargas/*.png ~/ejercicios-linux/imagenes/

# 4. Verifica el resultado
tree ~/ejercicios-linux/

🎯 Ejercicio 2: Búsqueda y filtrado
# 1. Crea un archivo de logs
echo "INFO: Inicio del sistema" > logs.txt
echo "ERROR: Conexión fallida" >> logs.txt
echo "INFO: Usuario logueado" >> logs.txt
echo "ERROR: Timeout" >> logs.txt

# 2. Busca solo las líneas de ERROR
grep "ERROR" logs.txt

# 3. Cuenta cuántos errores hay
grep -c "ERROR" logs.txt

# 4. Busca en todo el sistema archivos .conf que contengan "localhost"
sudo grep -r "localhost" /etc/*.conf 2>/dev/null

🎯 Ejercicio 3: Permisos y seguridad
# 1. Crea un script de backup
echo "#!/bin/bash" > backup.sh
echo "tar -czf backup-\$(date +%Y%m%d).tar.gz ~/documentos/" >> backup.sh

# 2. Hazlo ejecutable para todos
chmod +x backup.sh

# 3. Asegúrate de que solo tú puedas leer tu carpeta .ssh
mkdir -p ~/.ssh
chmod 700 ~/.ssh

# 4. Crea un archivo secreto
echo "mi_password_123" > ~/secreto.txt
chmod 600 ~/secreto.txt

🎯 Ejercicio 4: Monitoreo del sistema
# 1. Muestra los 5 procesos que más CPU consumen
ps aux --sort=-%cpu | head -6

# 2. Muestra los 5 procesos que más memoria consumen
ps aux --sort=-%mem | head -6

# 3. Monitorea tu uso de disco
df -h

# 4. Encuentra archivos grandes en tu home
find ~ -type f -size +50M 2>/dev/null -exec ls -lh {} \;

🎯 Ejercicio 5: Pipeline y redirección
# 1. Cuenta cuántos archivos .txt hay en tu home
find ~ -name "*.txt" | wc -l

# 2. Lista todos los usuarios y guarda en un archivo
cut -d: -f1 /etc/passwd > usuarios.txt

# 3. Ordena usuarios alfabéticamente
sort usuarios.txt

# 4. Muestra los 10 procesos más pesados y los guarda en un archivo
ps aux --sort=-%mem | head -11 > procesos_pesados.txt

⭐ Desafío final
# Crea un script que haga lo siguiente:
# 1. Crea una carpeta de backup en /tmp/backup
# 2. Encuentra todos los archivos .conf modificados en los últimos 7 días
# 3. Los comprime en backup-configs.tar.gz
# 4. Solo tú puedes leer el archivo de backup

# Solución:
cat > backup_configs.sh << 'EOF'
#!/bin/bash
mkdir -p /tmp/backup
find /etc -name "*.conf" -mtime -7 -exec cp {} /tmp/backup/ \;
tar -czf backup-configs.tar.gz /tmp/backup/
chmod 600 backup-configs.tar.gz
rm -rf /tmp/backup
echo "Backup creado: backup-configs.tar.gz"
EOF

chmod +x backup_configs.sh
./backup_configs.sh
