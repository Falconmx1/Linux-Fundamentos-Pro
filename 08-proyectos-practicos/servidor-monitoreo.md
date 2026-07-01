```markdown
# Proyecto: Sistema de Monitoreo

## 🎯 Objetivo

Implementar un sistema de monitoreo completo con Prometheus y Grafana.

---

## 📋 Requisitos

- Ubuntu/Debian
- Docker instalado
- Conexión a internet

---

## 🔧 Paso 1: Instalación con Docker

### docker-compose.yml para monitoreo
```yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    networks:
      - monitoring

volumes:
  prometheus_data:
  grafana_data:

networks:
  monitoring:
Configuración de Prometheus
yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node'
    static_configs:
      - targets: ['host.docker.internal:9100']
📊 Paso 2: Instalar Node Exporter

# Instalar Node Exporter
wget https://github.com/prometheus/node_exporter/releases/latest/download/node_exporter-linux-amd64.tar.gz
tar -xzf node_exporter-linux-amd64.tar.gz
sudo mv node_exporter-linux-amd64/node_exporter /usr/local/bin/

# Crear servicio systemd
sudo cat > /etc/systemd/system/node_exporter.service << 'EOF'
[Unit]
Description=Node Exporter
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Iniciar
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
📈 Paso 3: Configurar Grafana
bash
# Acceder a Grafana
# http://localhost:3000
# Usuario: admin
# Contraseña: admin

# Agregar fuente de datos:
# 1. Configuration → Data Sources
# 2. Add data source → Prometheus
# 3. URL: http://prometheus:9090

# Importar dashboard:
# 1. Dashboards → Import
# 2. ID: 1860 (Node Exporter Full)
🧪 Script de monitoreo automático

#!/bin/bash
# monitoring-setup.sh - Setup completo de monitoreo

echo "=== CONFIGURANDO SISTEMA DE MONITOREO ==="

# Crear directorios
mkdir -p ~/monitoring
cd ~/monitoring

# Crear docker-compose.yml
cat > docker-compose.yml << 'EOF'
version: '3.8'
services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana_data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    networks:
      - monitoring

volumes:
  prometheus_data:
  grafana_data:

networks:
  monitoring:
EOF

# Crear prometheus.yml
cat > prometheus.yml << 'EOF'
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['host.docker.internal:9100']
EOF

# Iniciar servicios
docker-compose up -d

echo "✅ Monitoreo configurado"
echo "📊 Grafana: http://localhost:3000 (admin/admin)"
echo "📈 Prometheus: http://localhost:9090"
