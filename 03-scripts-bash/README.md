# 🐚 Scripts en Bash

Bienvenido al mundo de la automatización con Bash scripting. Aquí aprenderás a crear scripts poderosos para automatizar tareas en Linux.

---

## 🎯 Objetivos de aprendizaje

- Entender la sintaxis básica de Bash
- Crear scripts funcionales desde cero
- Manejar variables, condicionales y bucles
- Trabajar con funciones y arrays
- Automatizar tareas del sistema
- Implementar buenas prácticas en scripting

---

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `introduccion-bash.md` | Fundamentos y primeros scripts |
| `variables-tipos.md` | Variables, tipos y operaciones |
| `condicionales-bucles.md` | if, case, for, while, until |
| `funciones-parametros.md` | Funciones y paso de parámetros |
| `entrada-salida.md` | read, echo, printf y redirecciones |
| `manejo-errores.md` | Códigos de salida, trap y debugging |
| `buenas-practicas.md` | Estilo, seguridad y optimización |
| `ejercicios-practicos.md` | Proyectos prácticos |

---

## 🚀 Primer script

### Hola Mundo en Bash
```bash
#!/bin/bash
# Mi primer script

echo "¡Hola, Linux!"

Guardar y ejecutar
# Guarda como hola.sh
nano hola.sh

# Dale permisos de ejecución
chmod +x hola.sh

# Ejecuta
./hola.sh

📋 Buenas prácticas esenciales
1-Siempre usa shebang: #!/bin/bash o #!/usr/bin/env bash

2-Comenta tu código: Explica qué hace cada parte

3-Usa variables con mayúsculas: NOMBRE="Juan"

4-Siempre entrecomilla variables: "$variable"

5-Verifica errores: Usa set -e o verifica códigos de salida

