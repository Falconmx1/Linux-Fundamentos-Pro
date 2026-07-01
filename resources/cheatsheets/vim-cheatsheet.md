```markdown
# Vim Cheatsheet - Guía Rápida

## 📋 Modos de Vim

| Modo | Descripción | Tecla para entrar |
|------|-------------|-------------------|
| **Normal** | Navegación y comandos | `Esc` |
| **Insert** | Escritura de texto | `i`, `a`, `o` |
| **Visual** | Selección de texto | `v`, `V`, `Ctrl+v` |
| **Command** | Comandos | `:` |

---

## 🚀 Navegación

### Básica
```vim
h       # Izquierda
j       # Abajo
k       # Arriba
l       # Derecha
w       # Siguiente palabra
b       # Palabra anterior
0       # Inicio de línea
$       # Fin de línea
gg      # Inicio del archivo
G       # Fin del archivo
Ctrl+d  # Bajar media página
Ctrl+u  # Subir media página
Avanzada
vim
{       # Párrafo anterior
}       # Siguiente párrafo
H       # Inicio de la pantalla
M       # Mitad de la pantalla
L       # Final de la pantalla
%       # Ir al paréntesis correspondiente
✏️ Edición
Insertar texto
vim
i       # Insertar antes del cursor
I       # Insertar al inicio de línea
a       # Insertar después del cursor
A       # Insertar al final de línea
o       # Nueva línea abajo
O       # Nueva línea arriba
Eliminar
vim
x       # Eliminar carácter
dw      # Eliminar palabra
dd      # Eliminar línea
d$      # Eliminar hasta fin de línea
d0      # Eliminar hasta inicio de línea
D       # Eliminar hasta fin de línea
Copiar y pegar
vim
yy      # Copiar línea
yw      # Copiar palabra
y$      # Copiar hasta fin de línea
p       # Pegar después del cursor
P       # Pegar antes del cursor
"+y     # Copiar al portapapeles del sistema
"+p     # Pegar del portapapeles del sistema
Cambiar
vim
cw      # Cambiar palabra
cc      # Cambiar línea
C       # Cambiar hasta fin de línea
r       # Reemplazar carácter
R       # Modo reemplazo
~       # Cambiar mayúscula/minúscula
🔍 Búsqueda
vim
/texto          # Buscar hacia adelante
?texto          # Buscar hacia atrás
n               # Siguiente coincidencia
N               # Coincidencia anterior
*               # Buscar palabra bajo cursor
#               # Buscar palabra bajo cursor (atrás)
🔄 Reemplazo
vim
:s/viejo/nuevo          # Reemplazar primera ocurrencia en línea
:s/viejo/nuevo/g        # Reemplazar todas en línea
:%s/viejo/nuevo/g       # Reemplazar en todo el archivo
:%s/viejo/nuevo/gc      # Reemplazar con confirmación
📂 Archivos
vim
:w              # Guardar
:w archivo      # Guardar como
:q              # Salir
:wq             # Guardar y salir
:x              # Guardar y salir
:q!             # Salir sin guardar
:e archivo      # Abrir archivo
:e!             # Recargar archivo
:sp archivo     # Dividir pantalla horizontal
:vsp archivo    # Dividir pantalla vertical
Ctrl+w w        # Cambiar entre paneles
⚙️ Configuración básica (~/.vimrc)
vim
" Configuración básica
set number              " Números de línea
set relativenumber      " Números relativos
set tabstop=4           " Tabulación a 4 espacios
set shiftwidth=4        " Indentación a 4 espacios
set expandtab           " Usar espacios en lugar de tabs
set autoindent          " Auto indentación
set smartindent         " Indentación inteligente
set hlsearch            " Resaltar búsquedas
set incsearch           " Búsqueda incremental
set ignorecase          " Ignorar mayúsculas en búsqueda
set smartcase           " Búsqueda sensible si hay mayúsculas
set wrap                " Ajuste de línea
set linebreak           " Ajuste en palabras

" Colores
syntax on               " Resaltado de sintaxis
colorscheme desert      " Esquema de colores
🧪 Atajos útiles
vim
.       # Repetir último comando
u       # Deshacer
Ctrl+r  # Rehacer
J       # Unir líneas
>>      # Indentar a la derecha
<<      # Indentar a la izquierda
==      # Auto indentar línea
=G      # Auto indentar hasta final
