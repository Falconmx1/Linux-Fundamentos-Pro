```markdown
# Filtros y Búsqueda Avanzada

## 🔍 grep - Buscar texto

### Uso básico
```bash
grep "texto" archivo.txt        # Busca texto en archivo
grep "error" *.log              # Busca en todos los .log
grep -i "linux" archivo.txt     # Búsqueda insensible a mayúsculas
grep -r "function" src/         # Busca recursivo en directorios
grep -v "excluir" archivo.txt   # Muestra líneas que NO contienen
grep -n "texto" archivo.txt     # Muestra número de línea
grep -c "texto" archivo.txt     # Cuenta coincidencias

Expresiones regulares básicas
grep "^start" archivo.txt       # Líneas que empiezan con "start"
grep "end$" archivo.txt         # Líneas que terminan con "end"
grep "[0-9]" archivo.txt        # Líneas con números
grep "^[A-Z]" archivo.txt       # Líneas que empiezan con mayúscula

📂 find - Buscar archivos
Uso básico
find . -name "*.txt"            # Busca archivos .txt
find /home -type f -size +10M   # Archivos mayores a 10MB
find . -type d -name "src"      # Directorios llamados "src"
find . -mtime -7                # Archivos modificados en los últimos 7 días
find . -empty                   # Archivos/directorios vacíos

Acciones con find
find . -name "*.tmp" -delete    # Elimina archivos .tmp
find . -name "*.sh" -exec chmod +x {} \;  # Hace ejecutables scripts
find . -name "*.log" -exec mv {} logs/ \; # Mueve logs a carpeta

📊 sed - Editor de flujo
Reemplazo de texto
sed 's/viejo/nuevo/g' archivo   # Reemplaza "viejo" por "nuevo" (global)
sed -i 's/viejo/nuevo/g' archivo # Reemplaza en el mismo archivo
sed 's/^/ /' archivo.txt        # Añade espacio al inicio de cada línea
sed '/^$/d' archivo.txt         # Elimina líneas vacías

📈 awk - Procesamiento de datos
Uso básico
awk '{print $1}' archivo.txt    # Imprime primera columna
awk '{print $1, $3}' archivo.txt # Imprime columna 1 y 3
awk -F: '{print $1}' /etc/passwd # Usa ':' como separador
awk '{sum+=$1} END {print sum}' archivo.txt  # Suma columna 1

🧪 Ejemplos prácticos
# Buscar errores en logs de sistema
sudo grep -r "ERROR" /var/log/

# Buscar archivos grandes
find / -type f -size +100M 2>/dev/null

# Reemplazar texto en múltiples archivos
sed -i 's/localhost/127.0.0.1/g' *.conf

# Mostrar usuarios únicos en /etc/passwd
cut -d: -f1 /etc/passwd | sort | uniq

# Contar líneas, palabras y caracteres
wc -l archivo.txt   # Líneas
wc -w archivo.txt   # Palabras
wc -c archivo.txt   # Caracteres

# Extraer IPs de un archivo
grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' archivo.txt
