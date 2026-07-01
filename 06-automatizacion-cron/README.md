```markdown
# ⏰ Automatización con Cron y Systemd

Esta carpeta cubre la automatización de tareas en Linux usando cron, systemd timers y herramientas modernas de programación.

---

## 🎯 Objetivos de aprendizaje

- Dominar cron y crontab
- Configurar systemd timers
- Automatizar tareas de mantenimiento
- Monitorear trabajos programados
- Mejores prácticas en automatización

---

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `cron-fundamentos.md` | Fundamentos de cron |
| `crontab-avanzado.md` | Configuraciones avanzadas |
| `systemd-timers.md` | Timers en systemd |
| `anacron.md` | Tareas periódicas en sistemas no 24/7 |
| `tareas-automaticas.md` | Ejemplos de tareas comunes |
| `monitoreo-logs.md` | Monitoreo y logs de trabajos |
| `scripts-automacion.md` | Scripts de automatización |

---

## 🚀 Comandos esenciales de cron

```bash
# Editar crontab del usuario actual
crontab -e

# Ver crontab del usuario actual
crontab -l

# Editar crontab de otro usuario
sudo crontab -u usuario -e

# Ver logs de cron
sudo journalctl -u cron -f
grep CRON /var/log/syslog

# Sintaxis básica
* * * * * comando
│ │ │ │ │
│ │ │ │ └── Día de la semana (0-7, 0=domingo)
│ │ │ └──── Mes (1-12)
│ │ └────── Día del mes (1-31)
│ └──────── Hora (0-23)
└────────── Minuto (0-59)
📋 Ejemplos de tareas comunes

# Backup diario a las 2 AM
0 2 * * * /usr/local/bin/backup.sh

# Limpieza semanal los domingos a las 3 AM
0 3 * * 0 /usr/local/bin/cleanup.sh

# Actualizar sistema cada lunes a las 4 AM
0 4 * * 1 sudo apt update && sudo apt upgrade -y

# Reiniciar servicio cada hora
0 * * * * sudo systemctl restart mi-servicio

# Script cada 5 minutos
*/5 * * * * /usr/local/bin/monitor.sh

# Lunes a viernes a las 9 AM
0 9 * * 1-5 /usr/local/bin/trabajo.sh
