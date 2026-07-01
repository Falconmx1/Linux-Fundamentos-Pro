# Terminal Básica

## 🖥️ ¿Qué es la terminal?

La terminal (o consola) es tu interfaz principal con Linux. Aquí escribes comandos para controlar el sistema.

---

## ⌨️ Comandos esenciales

### Navegación
```bash
pwd                 # Muestra la ruta actual
ls                  # Lista archivos y carpetas
ls -la              # Lista detallada (incluye ocultos)
cd /ruta            # Cambia de directorio
cd ..               # Sube un nivel
cd ~                # Va a tu carpeta personal
cd -                # Va al directorio anterior

Creación y manipulación
mkdir nombre        # Crea una carpeta
touch archivo.txt   # Crea un archivo vacío
cp origen destino   # Copia archivos
mv origen destino   # Mueve o renombra
rm archivo          # Elimina archivo
rm -r carpeta       # Elimina carpeta (cuidado!)

Visualización
cat archivo         # Muestra contenido completo
less archivo        # Muestra paginado (presiona 'q' para salir)
head -n 10 archivo  # Muestra las primeras 10 líneas
tail -n 10 archivo  # Muestra las últimas 10 líneas

Ayuda
man comando         # Manual del comando
comando --help      # Ayuda rápida
whatis comando      # Descripción breve

🎯 Atajos de teclado útiles
Atajo	Función
Ctrl + C	Cancela el comando actual
Ctrl + L	Limpia la pantalla
Ctrl + A	Ir al inicio de la línea
Ctrl + E	Ir al final de la línea
Ctrl + U	Borra desde el cursor al inicio
Ctrl + K	Borra desde el cursor al final
Tab	Autocompletar comandos/rutas
↑ / ↓	Navegar por el historial

🧪 Ejercicio rápido
# 1. Mira dónde estás
pwd

# 2. Crea una carpeta llamada "mi-primer-proyecto"
mkdir mi-primer-proyecto

# 3. Entra a la carpeta
cd mi-primer-proyecto

# 4. Crea un archivo llamado "hola.txt"
touch hola.txt

# 5. Escribe algo en el archivo
echo "Mi primer archivo en Linux" > hola.txt

# 6. Verifica el contenido
cat hola.txt

# 7. Vuelve atrás
cd ..
