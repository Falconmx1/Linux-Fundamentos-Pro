```markdown
# Gestión de Procesos

## 📊 Monitoreo

### Ver procesos
```bash
ps                    # Procesos del usuario actual
ps aux                # Todos los procesos (detallado)
ps -ef                # Todos los procesos (formato estándar)
ps aux | grep nginx   # Buscar procesos específicos

Monitoreo en tiempo real
top                   # Monitor de procesos interactivo
htop                  # Versión mejorada (instalar si no está)

Ver uso de recursos
free -h               # Memoria RAM y swap
df -h                 # Espacio en disco
du -sh carpeta/       # Tamaño de carpeta
iotop                 # Uso de disco por proceso
nethogs               # Uso de red por proceso

⚙️ Gestión de procesos
Señales y control
kill PID              # Termina proceso (SIGTERM)
kill -9 PID           # Termina forzadamente (SIGKILL)
kill -15 PID          # Termina graciosamente (SIGTERM)
pkill nombre          # Mata procesos por nombre
killall nombre        # Mata todos los procesos con ese nombre

Prioridad
nice -n 10 comando    # Ejecuta con prioridad baja
renice 5 -p PID       # Cambia prioridad en ejecución

Trabajos en segundo plano
comando &             # Ejecuta en segundo plano
jobs                  # Lista trabajos en segundo plano
fg %1                 # Trae trabajo al frente
bg %1                 # Reanuda trabajo en segundo plano
Ctrl+Z                # Suspende proceso actual
disown                # Desvincula proceso de la terminal

🧵 Procesos en detalle
Información de procesos
# /proc es un sistema de archivos virtual con info de procesos
cat /proc/cpuinfo     # Información del CPU
cat /proc/meminfo     # Información de memoria
cat /proc/version     # Versión del kernel

Procesos comunes
systemd               # Inicia todos los servicios (PID 1)
init                  # Sistema de inicio tradicional
cron                  # Tareas programadas
sshd                  # Servidor SSH
nginx/apache2         # Servidores web

🧪 Ejemplo práctico
# 1. Ejecuta un proceso en segundo plano
sleep 1000 &

# 2. Ve los procesos
ps aux | grep sleep

# 3. Mata el proceso
kill %1    # O kill -9 <PID>

# 4. Monitorea el sistema
htop

# 5. Busca procesos que consumen mucha CPU
ps aux --sort=-%cpu | head -10
