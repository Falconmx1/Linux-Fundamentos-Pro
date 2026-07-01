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

Copiar
cp origen destino           # Copia archivo
cp -r origen destino        # Copia directorio recursivo
cp -i origen destino        # Pregunta antes de sobrescribir
cp -v origen destino        # Muestra progreso
cp -u origen destino        # Copia solo si origen es más nuevo

Mover/Renombrar
mv origen destino           # Mueve archivo/directorio
mv viejo.txt nuevo.txt      # Renombra archivo
mv carpeta/ nueva-carpeta/  # Renombra directorio
mv -i origen destino        # Pregunta antes de sobrescribir

Eliminar
rm archivo.txt              # Elimina archivo
rm -r carpeta/              # Elimina directorio (cuidado)
rm -rf carpeta/             # Elimina sin confirmación (MUCHO CUIDADO)
rm -i archivo.txt           # Pregunta antes de eliminar
rmdir carpeta_vacia/        # Elimina directorio vacío

Enlaces
ln -s original enlace       # Crea enlace simbólico
ln original enlace          # Crea enlace físico (hard link)

📊 Comparativa de comandos
Acción	Comando	Ejemplo
Crear archivo	touch	touch data.txt
Crear carpeta	mkdir	mkdir datos
Copiar archivo	cp	cp data.txt backup/
Mover/renombrar	mv	mv data.txt datos/
Eliminar archivo	rm	rm data.txt
Eliminar carpeta	rm -r	rm -r datos/

⚠️ Consejos de seguridad
1-Siempre usa -i cuando tengas dudas

2-Nunca ejecutes rm -rf /* (borraría todo el sistema)

3-Usa ls antes de rm para confirmar lo que vas a eliminar

4-Crea backups antes de operaciones masivas

🧪 Ejercicio práctico
# Crea una estructura de proyecto
mkdir -p proyecto-linux/{src,bin,docs,logs}
cd proyecto-linux

# Crea archivos
touch src/main.sh
touch docs/README.md
echo "Hola" > logs/info.log

# Haz backups
cp -r src/ bin/

# Verifica
ls -la
tree
