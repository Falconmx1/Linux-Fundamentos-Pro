```markdown
# Funciones y Parámetros en Bash

## 🎯 Funciones

### Definición básica
```bash
# Método 1
function saludar() {
    echo "¡Hola, mundo!"
}

# Método 2 (más común)
saludar() {
    echo "¡Hola, mundo!"
}

# Llamar función
saludar
Funciones con parámetros
bash
saludar_persona() {
    local nombre="$1"    # $1 = primer parámetro
    local edad="$2"      # $2 = segundo parámetro
    echo "Hola $nombre, tienes $edad años"
}

saludar_persona "Juan" 30
Funciones con retorno

# Retornar código de salida (0 = éxito, 1-255 = error)
comprobar_archivo() {
    if [ -f "$1" ]; then
        return 0
    else
        return 1
    fi
}

if comprobar_archivo "/etc/passwd"; then
    echo "El archivo existe"
fi
Variables locales

mi_funcion() {
    local variable_local="Solo en la función"
    variable_global="Visible en todo el script"
    echo "$variable_local"
}

mi_funcion
echo "$variable_global"    # Funciona
# echo "$variable_local"   # ❌ No funciona (fuera de ámbito)
📥 Parámetros de script
Variables especiales
Variable	Descripción
$0	Nombre del script
$1, $2, ...	Parámetros posicionales
$#	Número de parámetros
$@	Todos los parámetros como array
$*	Todos los parámetros como string
$$	PID del script
$?	Código de salida del último comando
Ejemplo de procesamiento de parámetros

#!/bin/bash
# procesar.sh - Procesa parámetros

echo "Script: $0"
echo "Parámetros: $#"
echo "Primer parámetro: $1"
echo "Segundo parámetro: $2"
echo "Todos los parámetros: $@"

# Verificar si hay parámetros
if [ $# -eq 0 ]; then
    echo "Uso: $0 <parametro1> <parametro2>"
    exit 1
fi
Procesamiento avanzado (getopts)

#!/bin/bash
# opciones.sh - Script con opciones estilo CLI

while getopts "f:n:h" opcion; do
    case $opcion in
        f)
            archivo="$OPTARG"
            echo "Archivo: $archivo"
            ;;
        n)
            nombre="$OPTARG"
            echo "Nombre: $nombre"
            ;;
        h)
            echo "Uso: $0 [-f archivo] [-n nombre]"
            exit 0
            ;;
        \?)
            echo "Opción inválida: -$OPTARG"
            exit 1
            ;;
    esac
done
🧪 Ejemplo integrador

#!/bin/bash
# backup-tool.sh - Herramienta de backup

# Colores
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m'

# Funciones de utilidad
log_info() { echo -e "${GREEN}[INFO]${NC} $1"; }
log_error() { echo -e "${RED}[ERROR]${NC} $1" >&2; }
log_warn() { echo -e "${YELLOW}[WARN]${NC} $1"; }

# Función principal
crear_backup() {
    local origen="$1"
    local destino="$2"
    local fecha=$(date +%Y%m%d_%H%M%S)
    local archivo_backup="backup_${fecha}.tar.gz"
    
    if [ ! -d "$origen" ]; then
        log_error "El directorio origen no existe: $origen"
        return 1
    fi
    
    log_info "Creando backup de $origen..."
    tar -czf "${destino}/${archivo_backup}" -C "$origen" .
    
    if [ $? -eq 0 ]; then
        log_info "Backup creado exitosamente: ${archivo_backup}"
        return 0
    else
        log_error "Error al crear el backup"
        return 1
    fi
}

# Función de ayuda
mostrar_ayuda() {
    cat << EOF
Uso: $0 [OPCIONES]

Opciones:
    -s DIR     Directorio origen (obligatorio)
    -d DIR     Directorio destino (obligatorio)
    -h         Mostrar esta ayuda

Ejemplo:
    $0 -s /home/usuario/documentos -d /backup/
EOF
}

# Procesar argumentos
while getopts "s:d:h" opt; do
    case $opt in
        s) ORIGEN="$OPTARG" ;;
        d) DESTINO="$OPTARG" ;;
        h) mostrar_ayuda; exit 0 ;;
        *) mostrar_ayuda; exit 1 ;;
    esac
done

# Validar argumentos
if [ -z "$ORIGEN" ] || [ -z "$DESTINO" ]; then
    log_error "Faltan argumentos obligatorios"
    mostrar_ayuda
    exit 1
fi

# Ejecutar
crear_backup "$ORIGEN" "$DESTINO"
