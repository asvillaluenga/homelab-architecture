# Homelab Architecture

Documentación técnica de mi infraestructura doméstica autogestionada.  
El objetivo de este proyecto es demostrar diseño de red, segmentación, monitorización y despliegue de servicios en entornos híbridos.

---

## Infraestructura General

### Núcleo de Infraestructura

**Nodo Overwatch (Raspberry Pi 4 – 4GB | SSD 120GB)**  
_Actúa como Security & Monitoring Gateway Node._  
Gestiona la defensa de red en tiempo real, bloqueo de anuncios, el DNS recursivo y la automatización de seguridad.  
**Servicios:** Docker, Portainer, WatchTower, Uptime Kuma, NetAlertX, CrowdSec, Glances, Fail2Ban, Homarr, Unbound, Pi-hole (2.5M+ dominios bloqueados), PiVPN (WireGuard).

**Nodo Sentinel HA (Raspberry Pi Zero W)**  
_Nodo secundario dedicado a alta disponibilidad (High Availability) para redundancia de DNS y AD-Blocker._  
**Servicios:** Pi-hole + Unbound para DNS recursivo redundante y enrutamiento seguro de respaldo.

---

### Centro de Virtualización y Orquestación

**Atlas Core (Intel i7-9850H | 64GB RAM | NVMe 1TB Gen4)**  
_Servidor principal de virtualización y orquestación, ejecutando Proxmox VE._  
Opera como hipervisor central, alojando múltiples máquinas virtuales y contenedores para infraestructura crítica y automatización.  

**Stack principal:** Proxmox VE, Docker, Portainer, Wazuh, NGINX (Reverse Proxy), Grafana, Prometheus, Immich, Home Assistant, WatchTower.  

**Máquinas Virtuales:**  
- **Windows Server 2025** → Laboratorio empresarial, Active Directory y servicios de automatización.  
- **Kali Linux** → Entorno de simulación de amenazas, pentesting y análisis forense.  
- **Ubuntu Server** → Stack de seguridad y monitorización, domótica y respaldo de imágenes y videos.
 
**Entornos auxiliares dentro de Atlas Core:**  
Laboratorios de simulación avanzada (malware, ciberataques controlados y pruebas de seguridad) en entornos aislados para formación y análisis de respuesta ante incidentes.

---

### Capa de Medios y Automatización

** Helios Media Server (Intel i5-8365U | 16GB RAM | NVMe 1TB + DAS 64TB)**  
_Servidor multimedia y de automatización, diseñado para disponibilidad continua y gestión de contenido avanzada._  
**Servicios:** Plex, Radarr, Sonarr, Lidarr, Bazarr, Readarr, mylar3, qBittorrent, Tube Archivist, Tautulli, Overseerr, Docker, Portainer, WatchTower.

---

### Inteligencia Artificial y Experimentación

**Hermes Node (Mac Mini M2 – 16GB RAM)**  
_Nodo de I+D destinado a experimentación con modelos de lenguaje._  
**Servicios:** LM Studio ejecutando modelos **LLM 7B personalizados**, pruebas de razonamiento y benchmarking.

**Aether Forge (Ryzen 9 3900X + RTX 3070)**  
_Estación de trabajo dedicada a IA generativa y síntesis visual._  
**Uso:** Entorno local de **Stable Diffusion** para ingeniería de prompts, entrenamiento de modelos y optimización de flujos de generación de imágenes.

---

### Infraestructura de Red

- **DNS Stack:** Resolución dual con Pi-hole + Unbound recursivo.  
- **Capa VPN:** WireGuard (PiVPN) con perfiles de cliente y enrutamiento centralizado + TailScale.  
- **Malla de Monitoreo:** Grafana, Uptime Kuma, MySpeed, NetAlertX, CrowdSec y Glances.  
- **Automatización:** WatchTower para actualización continua de contenedores, con scripts de PowerShell y Bash para orquestación y mantenimiento.

---


---

## Topología de Red

![Network Diagram](./docs/homelab-network-diagram.md)

- Red principal segmentada en subredes (IoT, LAN, Media, VPN, Guests)
- DNS interno con resolución recursiva y caché local
- Enrutamiento seguro vía PiVPN
- Monitorización continua del estado de red y rendimiento

---

## Seguridad Implementada

- DNS over TLS con Unbound  
- Bloqueo publicitario y de rastreadores (Pi-hole + listas personalizadas)  
- Aislamiento de contenedores por stack funcional  
- Backups automáticos y logs centralizados  
- Firewall basado en reglas iptables y UFW

---

## Tecnologías Clave

| Categoría | Herramientas |
|------------|--------------|
| Virtualización | Docker, Portainer |
| Seguridad | PiVPN, Unbound, Firewall UFW |
| Monitorización | NetAlertX, Uptime Kuma, Tautulli |
| OS | Ubuntu Server, Windows Server, Kali Linux, Windows 11 Pro |
| Hardware | Raspberry Pi, Dell Latitude, Mac Mini, 2 custom Ryzen PCs |

---

 **Documentación complementaria:**  
- [Hardware Inventory](./docs/hardware-inventory.md)  
- [Docker Stack Overview](./docs/docker-stack-overview.md)

> _"Si no lo puedes medir, no lo puedes mejorar."_
