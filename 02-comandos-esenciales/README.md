```markdown
# ⚡ Comandos Esenciales de Linux

Esta carpeta contiene los comandos fundamentales que todo usuario de Linux debe dominar.

---

## 🎯 Objetivos de aprendizaje

- Dominar la manipulación de archivos y directorios
- Gestionar permisos y usuarios
- Entender el flujo de datos (pipe, redirección)
- Buscar y filtrar información
- Monitorear el sistema

---

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `manipulacion-archivos.md` | Crear, copiar, mover, eliminar |
| `permisos-usuarios.md` | chmod, chown, gestión de usuarios |
| `filtros-busqueda.md` | grep, find, sed, awk |
| `gestion-procesos.md` | ps, top, kill, jobs |
| `redireccion-pipes.md` | |, >, >>, <, tee |
| `comandos-utiles.md` | wget, curl, tar, gzip, alias |
| `ejercicios-practicos.md` | Ejercicios integradores |

---

## 🚀 Comandos rápidos para empezar

```bash
# Explorar archivos
ls -la              # Lista completa
tree                # Muestra árbol de directorios (instalar si no está)

# Leer archivos
cat archivo.txt     # Contenido completo
less archivo.txt    # Contenido paginado

# Buscar contenido
grep "texto" *.txt  # Busca en todos los .txt

# Descargar archivos
wget https://ejemplo.com/archivo.zip
curl -O https://ejemplo.com/archivo.zip

# Comprimir/descomprimir
tar -czf archivo.tar.gz carpeta/
tar -xzf archivo.tar.gz

# Permisos
chmod +x script.sh   # Hace ejecutable un script
sudo chown user:user archivo  # Cambia propietario

# Procesos
ps aux              # Lista todos los procesos
top                 # Monitor de procesos en tiempo real

# Red
ping google.com     # Prueba de conectividad
ss -tulpn           # Puertos abiertos
