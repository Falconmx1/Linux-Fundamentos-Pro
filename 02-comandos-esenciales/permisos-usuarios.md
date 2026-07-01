```markdown
# Permisos y Usuarios en Linux

## 👤 Usuarios

### Información de usuarios
```bash
whoami              # Usuario actual
id                  # ID de usuario y grupos
groups              # Grupos del usuario actual
cat /etc/passwd     # Lista todos los usuarios
cat /etc/group      # Lista todos los grupos

Gestión de usuarios (sudo)
sudo useradd nombre      # Crear usuario
sudo userdel nombre      # Eliminar usuario
sudo passwd nombre       # Cambiar contraseña
sudo usermod -aG grupo usuario  # Añadir usuario a grupo
sudo deluser usuario grupo       # Quitar usuario de grupo

🔐 Permisos de archivos
Entendiendo los permisos
ls -la archivo
# -rw-r--r-- 1 usuario grupo 1024 Ene 1 12:00 archivo
# ^^^^^^^^^  ^^^^^^^ ^^^^^
# Permisos   Usuario Grupo

Formato de permisos (rwx)
Símbolo	Significado
r	Lectura (read)
w	Escritura (write)
x	Ejecución (execute)
-	Sin permiso

Grupos de permisos
Usuario (owner) - los primeros 3

Grupo - los siguientes 3

Otros (others) - los últimos 3

Números (notación octal)
Número	Permisos
7	rwx (4+2+1)
6	rw- (4+2+0)
5	r-x (4+0+1)
4	r-- (4+0+0)
0	--- (0+0+0)
🛠️ Comandos de permisos
chmod - Cambiar permisos
# Notación simbólica
chmod u+x script.sh     # Añade ejecución al usuario
chmod go-rw archivo.txt # Quita lectura/escritura a grupo y otros
chmod a+x script.sh     # Añade ejecución a todos

# Notación numérica
chmod 755 script.sh     # rwxr-xr-x (usuario: todos, grupo/otros: leer+ejecutar)
chmod 644 archivo.txt   # rw-r--r-- (usuario: leer+escribir, grupo/otros: solo leer)
chmod 600 secreto.txt   # rw------- (solo usuario puede leer/escribir)

chown - Cambiar propietario
chown usuario archivo           # Cambia propietario
chown usuario:grupo archivo     # Cambia propietario y grupo
chown -R usuario:grupo carpeta/ # Cambia recursivamente

chgrp - Cambiar grupo
chgrp grupo archivo             # Cambia solo el grupo
chgrp -R grupo carpeta/         # Cambia recursivamente

📊 Tabla de permisos comunes
Permisos (num)	Permisos (texto)	Uso común
644	rw-r--r--	Archivos de texto normales
755	rwxr-xr-x	Ejecutables, scripts
600	rw-------	Archivos sensibles (claves SSH)
700	rwx------	Directorios privados
777	rwxrwxrwx	EVITAR - demasiado permisivo

🔑 Archivos especiales
/etc/sudoers
sudo visudo         # Edita el archivo de sudoers (USAR SIEMPRE visudo)

~/.ssh
mkdir -p ~/.ssh
chmod 700 ~/.ssh                # Directorio privado
touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys # Archivo de claves

🧪 Ejercicio práctico
# 1. Crea un script
echo "#!/bin/bash" > mi-script.sh
echo "echo Hola" >> mi-script.sh

# 2. Hazlo ejecutable
chmod +x mi-script.sh

# 3. Ejecútalo
./mi-script.sh

# 4. Cambia permisos para que solo tú puedas ejecutarlo
chmod 700 mi-script.sh

# 5. Verifica
ls -la mi-script.sh
