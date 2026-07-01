```markdown
# Introducción a Bash Scripting

## 🧠 ¿Qué es Bash?

**Bash** (Bourne Again SHell) es el intérprete de comandos más popular en Linux. Con Bash podemos crear scripts (programas) que ejecutan secuencias de comandos automáticamente.

---

## 📝 Estructura básica de un script

```bash
#!/bin/bash
# Comentarios comienzan con #
# Este es un script de ejemplo

echo "¡Ejecutando mi primer script!"

Componentes clave
Shebang: #!/bin/bash le dice al sistema qué intérprete usar

Comentarios: # para documentar tu código

Comandos: Cualquier comando de Linux funciona aquí

🎬 Ejecutar un script
Métodos de ejecución
# Método 1: Directo (requiere permisos de ejecución)
chmod +x script.sh
./script.sh

# Método 2: Usando bash explícitamente
bash script.sh

# Método 3: Con source (ejecuta en el shell actual)
source script.sh
. script.sh

Diferencia entre métodos
Método	Ejecuta en subshell	Variables persistentes
./script.sh	✅ Sí	❌ No
bash script.sh	✅ Sí	❌ No
source script.sh	❌ No	✅ Sí

🎨 Colores y formato en Bash
# Colores básicos
echo -e "\033[31mRojo\033[0m"
echo -e "\033[32mVerde\033[0m"
echo -e "\033[33mAmarillo\033[0m"
echo -e "\033[34mAzul\033[0m"
echo -e "\033[1mNegrita\033[0m"

# Funciones útiles
function mensaje_ok() {
    echo -e "\033[32m[✔] $1\033[0m"
}

function mensaje_error() {
    echo -e "\033[31m[✘] $1\033[0m" >&2
}

function mensaje_info() {
    echo -e "\033[34m[→] $1\033[0m"
}

# Uso
mensaje_ok "Todo correcto"
mensaje_error "Algo falló"
mensaje_info "Procesando..."

🧪 Ejercicio inicial
#!/bin/bash
# script-info.sh - Muestra información del sistema

echo "=== INFORMACIÓN DEL SISTEMA ==="
echo "Usuario: $(whoami)"
echo "Hostname: $(hostname)"
echo "Sistema: $(uname -a)"
echo "Fecha: $(date)"
echo "Directorio: $(pwd)"
echo "================================"
