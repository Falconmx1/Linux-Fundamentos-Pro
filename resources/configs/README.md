```markdown
# Configuraciones de Linux

Esta carpeta contiene archivos de configuración útiles para personalizar tu sistema Linux.

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `bashrc` | Configuración de bash |
| `vimrc` | Configuración de Vim |
| `sshd_config` | Configuración segura de SSH |
| `sysctl.conf` | Configuración del kernel |
| `ufw.rules` | Reglas de firewall |
| `crontab` | Tareas programadas |
| `nginx.conf` | Configuración de Nginx |
| `mysql.cnf` | Configuración de MySQL |
resources/configs/bashrc

# ~/.bashrc - Configuración de bash

# Aliases útiles
alias ll='ls -la'
alias la='ls -A'
alias l='ls -l'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias df='df -h'
alias du='du -h'
alias free='free -h'
alias mkdir='mkdir -pv'
alias cp='cp -iv'
alias mv='mv -iv'
alias rm='rm -iv'
alias c='clear'
alias h='history'
alias ports='ss -tulpn'
alias psg='ps aux | grep'
alias update='sudo apt update && sudo apt upgrade -y'

# Git aliases
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias gl='git log --oneline --graph'

# Docker aliases
alias dps='docker ps'
alias dpsa='docker ps -a'
alias dim='docker images'
alias drm='docker rm -f'
alias drmi='docker rmi'
alias dcu='docker-compose up -d'
alias dcd='docker-compose down'

# Variables de entorno
export EDITOR=vim
export PATH="$HOME/bin:/usr/local/bin:$PATH"

# Prompt personalizado
PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# Funciones útiles
mkcd() {
    mkdir -p "$1" && cd "$1"
}

extract() {
    if [ -f "$1" ]; then
        case "$1" in
            *.tar.bz2) tar xjf "$1" ;;
            *.tar.gz) tar xzf "$1" ;;
            *.bz2) bunzip2 "$1" ;;
            *.rar) unrar x "$1" ;;
            *.gz) gunzip "$1" ;;
            *.tar) tar xf "$1" ;;
            *.tbz2) tar xjf "$1" ;;
            *.tgz) tar xzf "$1" ;;
            *.zip) unzip "$1" ;;
            *.Z) uncompress "$1" ;;
            *.7z) 7z x "$1" ;;
            *) echo "No sé cómo extraer: $1" ;;
        esac
    else
        echo "'$1' no es un archivo válido"
    fi
}

backup() {
    cp "$1" "$1.backup.$(date +%Y%m%d_%H%M%S)"
}

# Cargar funciones adicionales
if [ -f ~/.bash_functions ]; then
    . ~/.bash_functions
fi
resources/configs/vimrc
vim
" ~/.vimrc - Configuración de Vim

" Básico
set nocompatible
syntax on
set number
set relativenumber
set showcmd
set showmode
set mouse=a

" Indentación
set tabstop=4
set shiftwidth=4
set expandtab
set autoindent
set smartindent

" Búsqueda
set hlsearch
set incsearch
set ignorecase
set smartcase

" Visual
set wrap
set linebreak
set scrolloff=5
set colorcolumn=80

" Archivos
set backup
set swapfile
set backupdir=~/.vim/backup
set directory=~/.vim/swap
set undodir=~/.vim/undo

" Atajos
nnoremap <leader>w :w<CR>
nnoremap <leader>q :q<CR>
nnoremap <leader>x :x<CR>
nnoremap <leader>e :Explore<CR>

" Autocomandos
autocmd BufNewFile *.sh 0r /usr/share/vim/vimfiles/templates/sh.tpl
autocmd BufNewFile *.py 0r /usr/share/vim/vimfiles/templates/py.tpl

" Tema
colorscheme desert
resources/configs/sshd_config
bash
# /etc/ssh/sshd_config - Configuración segura de SSH

# Puerto
Port 2222

# Protocolo
Protocol 2

# Autenticación
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM no

# Llaves
AuthorizedKeysFile .ssh/authorized_keys
HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_rsa_key

# Seguridad
MaxAuthTries 3
MaxSessions 5
ClientAliveInterval 300
ClientAliveCountMax 2

# Forwarding
AllowTcpForwarding no
X11Forwarding no

# Logging
LogLevel VERBOSE
SyslogFacility AUTH

# Usuarios permitidos
AllowUsers usuario1 usuario2
AllowGroups sudo ssh-users

# Cifrado
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
MACs hmac-sha2-512-etm@openssh.com
KexAlgorithms curve25519-sha256@libssh.org
