```markdown
# Condicionales y Bucles en Bash

## 🔀 Condicionales

### if-elif-else
```bash
if [ condición ]; then
    # código si condición es verdadera
elif [ otra_condición ]; then
    # código si otra_condición es verdadera
else
    # código si ninguna condición es verdadera
fi
Ejemplos prácticos
bash
# Comparación numérica
edad=18
if [ $edad -ge 18 ]; then
    echo "Eres mayor de edad"
else
    echo "Eres menor de edad"
fi

# Comparación de strings
nombre="Juan"
if [ "$nombre" = "Juan" ]; then
    echo "Hola Juan!"
fi

# Verificar archivos
if [ -f "/etc/passwd" ]; then
    echo "El archivo existe"
fi
Operadores de archivos
Operador	Significado
-f	Es un archivo regular
-d	Es un directorio
-e	Existe (archivo o directorio)
-r	Tiene permiso de lectura
-w	Tiene permiso de escritura
-x	Tiene permiso de ejecución
-s	No está vacío
case (switch)

case $variable in
    valor1)
        echo "Es valor1"
        ;;
    valor2|valor3)
        echo "Es valor2 o valor3"
        ;;
    *)
        echo "Otro valor"
        ;;
esac
🔄 Bucles
for loop

# Recorrer lista
for item in manzana pera naranja; do
    echo "Fruta: $item"
done

# Con secuencias
for i in {1..10}; do
    echo "Número: $i"
done

# Con incremento
for i in {1..10..2}; do
    echo "Número impar: $i"
done

# Con archivos
for archivo in *.txt; do
    echo "Procesando: $archivo"
done
while loop
bash
# Contador
contador=1
while [ $contador -le 5 ]; do
    echo "Contador: $contador"
    ((contador++))
done

# Leer archivo línea por línea
while IFS= read -r linea; do
    echo "Línea: $linea"
done < archivo.txt
until loop

# Similar a while pero ejecuta mientras la condición sea FALSA
contador=1
until [ $contador -gt 5 ]; do
    echo "Contador: $contador"
    ((contador++))
done
🧪 Ejemplos prácticos

#!/bin/bash
# menu.sh - Menú interactivo

echo "=== MENÚ PRINCIPAL ==="
echo "1. Información del sistema"
echo "2. Procesos en ejecución"
echo "3. Espacio en disco"
echo "4. Salir"

read -p "Elige una opción: " opcion

case $opcion in
    1)
        echo "=== INFORMACIÓN ==="
        uname -a
        ;;
    2)
        echo "=== PROCESOS ==="
        ps aux | head -10
        ;;
    3)
        echo "=== ESPACIO ==="
        df -h
        ;;
    4)
        echo "¡Hasta luego!"
        exit 0
        ;;
    *)
        echo "Opción inválida"
        ;;
esac
🧪 Ejemplo: Sumar números en un archivo

#!/bin/bash
# suma.sh - Suma números de un archivo

total=0
while read -r numero; do
    total=$((total + numero))
done < numeros.txt

echo "Suma total: $total"
