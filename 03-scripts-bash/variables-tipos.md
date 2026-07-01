```markdown
# Variables y Tipos en Bash

## 📦 Variables

### Declaración y asignación
```bash
# Sin espacios alrededor del =
nombre="Juan"
edad=25
ciudad="Madrid"

# Variables de solo lectura
readonly PI=3.14159

# Variables de entorno
export PATH="$HOME/bin:$PATH"

Uso de variables
# Siempre usa $ para acceder
echo "Hola, $nombre"
echo "Tienes $edad años"

# Entrecomillar variables (¡IMPORTANTE!)
echo "Vives en $ciudad"          # ✅ Bueno
echo "Vives en $ciudad"          # ✅ Bueno
echo Vives en $ciudad            # ❌ Malo (problemas con espacios)

Tipos de variables
# Strings
nombre="Juan"
frase="Hola, mundo!"

# Números (Bash no tiene tipos, todo es string)
edad=30
precio=19.99

# Arrays
frutas=("manzana" "pera" "naranja")
echo ${frutas[0]}   # manzana
echo ${frutas[@]}   # todos los elementos
echo ${#frutas[@]}  # número de elementos

# Arrays asociativos (diccionarios) - Bash 4+
declare -A usuario
usuario[nombre]="Juan"
usuario[edad]=30
echo ${usuario[nombre]}  # Juan

🔢 Operaciones numéricas
Aritmética básica
# Usando $((...))
suma=$((5 + 3))
resta=$((10 - 4))
multiplicacion=$((6 * 7))
division=$((20 / 4))
modulo=$((10 % 3))

# Incremento/Decremento
contador=0
((contador++))
((contador--))
contador=$((contador + 1))

Operadores comparativos
# Numéricos
[ $a -eq $b ]   # Igual
[ $a -ne $b ]   # Diferente
[ $a -gt $b ]   # Mayor que
[ $a -lt $b ]   # Menor que
[ $a -ge $b ]   # Mayor o igual
[ $a -le $b ]   # Menor o igual

# Strings
[ "$a" = "$b" ]     # Igual
[ "$a" != "$b" ]    # Diferente
[ -z "$a" ]         # Vacío
[ -n "$a" ]         # No vacío

📊 Expansión de variables
# Sustitución
archivo="documento.txt"
echo ${archivo%.txt}   # documento (quita extensión)
echo ${archivo##*.}    # txt (obtiene extensión)
echo ${archivo:0:5}    # docum (primeros 5 caracteres)

# Sustitución de texto
texto="Hola mundo"
echo ${texto/mundo/amigo}  # Hola amigo
echo ${texto//o/O}         # HOla mundO (todas las o)

# Valores por defecto
nombre=${1:-"desconocido"}  # Si no hay parámetro, usa "desconocido"

🧪 Ejemplo práctico
#!/bin/bash
# calculadora.sh - Calculadora simple

read -p "Ingresa el primer número: " num1
read -p "Ingresa el segundo número: " num2

echo "Suma: $((num1 + num2))"
echo "Resta: $((num1 - num2))"
echo "Multiplicación: $((num1 * num2))"
echo "División: $((num1 / num2))"
echo "Módulo: $((num1 % num2))"
