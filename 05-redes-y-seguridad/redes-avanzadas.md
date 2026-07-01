```markdown
# Redes Avanzadas en Linux

## 🧠 Conceptos avanzados

### Routing y tablas de ruteo
```bash
# Ver tabla de ruteo
ip route show
route -n

# Añadir ruta estática
sudo ip route add 192.168.2.0/24 via 192.168.1.1 dev eth0

# Añadir ruta por defecto
sudo ip route add default via 192.168.1.1

# Eliminar ruta
sudo ip route del 192.168.2.0/24
Bridge (puente de red)

# Instalar bridge-utils
sudo apt install bridge-utils

# Crear bridge
sudo brctl addbr br0

# Añadir interfaces al bridge
sudo brctl addif br0 eth0
sudo brctl addif br0 eth1

# Configurar IP al bridge
sudo ip addr add 192.168.1.100/24 dev br0
sudo ip link set br0 up
VLANs

# Crear interfaz VLAN
sudo ip link add link eth0 name eth0.10 type vlan id 10
sudo ip addr add 192.168.10.1/24 dev eth0.10
sudo ip link set eth0.10 up

# VLAN con /etc/network/interfaces
auto eth0.10
iface eth0.10 inet static
    address 192.168.10.1
    netmask 255.255.255.0
    vlan-raw-device eth0
📊 Network Namespaces (contenedores de red)

# Crear namespace
sudo ip netns add ns1

# Ver namespaces
sudo ip netns list

# Ejecutar comando en namespace
sudo ip netns exec ns1 bash

# Crear veth pair (cable virtual)
sudo ip link add veth0 type veth peer name veth1

# Mover veth1 al namespace
sudo ip link set veth1 netns ns1

# Configurar IPs
sudo ip addr add 10.0.0.1/24 dev veth0
sudo ip netns exec ns1 ip addr add 10.0.0.2/24 dev veth1
sudo ip link set veth0 up
sudo ip netns exec ns1 ip link set veth1 up

# Ping entre namespaces
sudo ip netns exec ns1 ping 10.0.0.1
🌐 Traffic Control (TC)

# Limitar ancho de banda
sudo tc qdisc add dev eth0 root handle 1: htb default 30
sudo tc class add dev eth0 parent 1: classid 1:1 htb rate 1mbit
sudo tc class add dev eth0 parent 1:1 classid 1:10 htb rate 1mbit

# Simular latencia
sudo tc qdisc add dev eth0 root netem delay 100ms

# Simular pérdida de paquetes
sudo tc qdisc add dev eth0 root netem loss 10%

# Eliminar reglas
sudo tc qdisc del dev eth0 root
🧪 Script de configuración de red avanzada

#!/bin/bash
# network-advanced.sh - Configuración avanzada de red

# Configurar bridge
setup_bridge() {
    local bridge_name="$1"
    local interfaces=("${@:2}")
    
    echo "Configurando bridge: $bridge_name"
    sudo brctl addbr "$bridge_name"
    
    for iface in "${interfaces[@]}"; do
        echo "  Añadiendo $iface al bridge"
        sudo brctl addif "$bridge_name" "$iface"
    done
    
    sudo ip link set "$bridge_name" up
    echo "✅ Bridge $bridge_name configurado"
}

# Configurar VLAN
setup_vlan() {
    local interface="$1"
    local vlan_id="$2"
    local ip_address="$3"
    local vlan_iface="${interface}.${vlan_id}"
    
    echo "Configurando VLAN $vlan_id en $interface"
    sudo ip link add link "$interface" name "$vlan_iface" type vlan id "$vlan_id"
    sudo ip addr add "$ip_address" dev "$vlan_iface"
    sudo ip link set "$vlan_iface" up
    echo "✅ VLAN $vlan_id configurada"
}

# Configurar routing
setup_routing() {
    local network="$1"
    local gateway="$2"
    
    echo "Añadiendo ruta: $network via $gateway"
    sudo ip route add "$network" via "$gateway"
}

# Ejemplo de uso
setup_bridge "br0" "eth0" "eth1"
setup_vlan "eth0" 10 "192.168.10.1/24"
setup_routing "192.168.2.0/24" "192.168.1.254"
