# 🐳 Virtualización y Contenedores

Esta carpeta cubre las tecnologías de virtualización y contenedores en Linux, incluyendo Docker, Podman, LXC/LXD, KVM y Vagrant.

---

## 🎯 Objetivos de aprendizaje

- Entender los conceptos de virtualización
- Dominar Docker y contenedores
- Gestionar contenedores con Podman
- Administrar LXC/LXD
- Implementar KVM y máquinas virtuales
- Usar Vagrant para entornos de desarrollo

---

## 📂 Contenido

| Archivo | Descripción |
|---------|-------------|
| `intro-virtualizacion.md` | Conceptos de virtualización |
| `docker-fundamentos.md` | Fundamentos de Docker |
| `docker-avanzado.md` | Docker avanzado y redes |
| `docker-compose.md` | Orquestación con Docker Compose |
| `podman.md` | Alternativa a Docker sin daemon |
| `lxc-lxd.md` | Contenedores de sistema |
| `kvm.md` | Máquinas virtuales con KVM |
| `vagrant.md` | Entornos de desarrollo reproducibles |
| `scripts-contenedores.md` | Scripts para contenedores |

---

## 🚀 Comandos esenciales

```bash
# Docker
docker ps
docker images
docker run -it ubuntu bash
docker build -t mi-imagen .

# Podman
podman run -it fedora bash
podman ps

# LXC/LXD
lxc list
lxc launch ubuntu:22.04 mi-contenedor
lxc exec mi-contenedor bash

# KVM
sudo virsh list --all
sudo virt-manager

# Vagrant
vagrant up
vagrant ssh
vagrant destroy
