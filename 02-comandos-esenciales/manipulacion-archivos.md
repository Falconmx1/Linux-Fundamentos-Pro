```markdown
# Manipulación de Archivos y Directorios

## 📂 Comandos básicos

### Creación
```bash
# Archivos
touch archivo.txt           # Crea archivo vacío
echo "contenido" > archivo  # Crea archivo con contenido
cat > archivo.txt           # Crea y escribe (Ctrl+D para guardar)

# Directorios
mkdir carpeta               # Crea directorio
mkdir -p ruta/completa      # Crea directorios anidados
mkdir -p proyecto/{src,bin,docs}  # Crea múltiples subdirectorios
