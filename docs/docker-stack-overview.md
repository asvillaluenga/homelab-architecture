# Docker Stack Overview

Panorama completo de los servicios contenedorizados dentro del Homelab, organizados por función, nodo y criticidad.  
La infraestructura está optimizada para resiliencia, automatización continua, seguridad reforzada y despliegue modular.

---

## 1. Arquitectura General del Stack Docker

El Homelab opera una infraestructura Docker distribuida en tres nodos principales:

| Nodo | Función | Carga Docker |
|------|---------|--------------|
| **Overwatch (RPi4)** | Seguridad, DNS, VPN, monitorización | Lightweight security & automation stack |
| **Atlas Core (Proxmox VM Ubuntu)** | Orquestación central, seguridad, automatización | Heavy services & backend stack |
| **Helios Server** | Media y automatización | Media Management Stack |

Toda la orquestación se hace mediante **Portainer**, ejecutándose tanto en Overwatch como en Helios.

---

## 2. Docker Stack por Nodo

---

## 2.1 Overwatch Node (Raspberry Pi 4 – 4GB)

_Node de seguridad, DNS, filtrado, automatización ligera y servicios always-on._

**Servicios Docker:**

| Servicio | Rol | Notas |
|----------|------|-------|
| **Pi-hole** | DNS filtering | Integración con Unbound |
| **Unbound** | DNS recursivo | DNS-over-TLS + caching |
| **PiVPN** | VPN WireGuard | Profile-based routing |
| **CrowdSec** | IPS/Anti-abuse | Filtro de patrones maliciosos |
| **Fail2Ban** | Hardening accesos | SSH + servicios expuestos |
| **NetAlertX** | Monitorización de red | Alertas en tiempo real |
| **Uptime Kuma** | Monitorización | Healthchecks internos/externos |
| **Glances** | Observabilidad del nodo | System metrics |
| **Homarr** | Dashboard central | Control y visibilidad |
| **WatchTower** | Auto-updates | Actualización automática de contenedores |
| **Portainer** | Orquestación | UI de gestión Docker |

---

## 2.2 Atlas Core (VM Ubuntu – Proxmox)

_Node principal de orquestación, backend, seguridad y servicios críticos._

**Servicios Docker:**

| Servicio | Rol |
|----------|------|
| **Wazuh** | SIEM central |
| **NGINX Proxy Manager** | Reverse proxy + SSL |
| **Grafana** | Visualización |
| **Prometheus** | Métricas |
| **Node Exporter** | Métricas de hardware |
| **Immich** | Gestión multimedia inteligente |
| **Home Assistant** | Domótica e integraciones |
| **WatchTower** | CI/CD de contenedores |
| **Portainer Agent** | Gestión desde Portainer Master |

---

## 2.3 Helios Media Server

_Node dedicado a gestión multimedia, automatización, indexación y content pipelines._

**Servicios Docker:**

| Servicio | Rol |
|----------|------|
| **Radarr** | Películas |
| **Sonarr** | Series |
| **Lidarr** | Música |
| **Bazarr** | Subtítulos |
| **Readarr** | Libros |
| **mylar3** | Cómics |
| **Tube Archivist** | Archivado YouTube |
| **Tautulli** | Analytics Plex |
| **Overseerr** | Requests |
| **Portainer** | Gestión local |
| **WatchTower** | Actualizaciones |

---

# 3. Seguridad del Stack Docker

Medidas implementadas según tu arquitectura:

- Contenedores aislados por nodo y función  
- NGINX Proxy Manager con certificados SSL automatizados  
- Fail2Ban integrado con jail personalizado  
- CrowdSec protegiendo accesos y patrones maliciosos  
- WireGuard para administración remota segura  
- Zero trust networking (VLANs separadas para IoT/Guest/Servers)  
- Contenedores críticos sin exposición pública directa  
- Regeneración automática de imágenes mediante WatchTower  

---

# 4. Monitorización y Observabilidad

**Pipelines activos:**

- Uptime Kuma → Healthchecks  
- Grafana → Dashboards unificados  
- Prometheus → Métricas y scraping  
- Glances → Observabilidad en nodos ARM  
- NetAlertX → Supervisión de red y alertas  
- Tautulli → Análisis de actividad Plex  

---

# 5. Flujos de CI/CD para Docker

**WatchTower Automation:**

- Auto-pull de imágenes  
- Auto-restart tras actualización  
- Notificaciones opcionales  
- Aplica a los contenedores de Overwatch, Atlas y Helios  

**Portainer Stacks:**

- Deploys reproducibles  
- Stacks versionados  
- Orquestación manual asistida por UI  

---

## 6. Docker Network Design

- Subredes aisladas por stack (seguridad, media, backend)
- Reverse Proxy centralizado
- NAT deshabilitado en nodos ARM para rutas limpias
- DNS interno con Pi-hole + Unbound
- VLAN2 como backbone principal de servicios

---

# 7. Diagram Overview (Mermaid)

```mermaid
flowchart TD

A[Overwatch RPi4] -->|Security Stack| S1[Pi-hole]
A --> S2[Unbound]
A --> S3[CrowdSec]
A --> S4[NetAlertX]
A --> S5[Uptime Kuma]
A --> S6[Portainer]

B[Atlas Core] -->|Backend Stack| B1[Wazuh]
B --> B2[NGINX Proxy]
B --> B3[Grafana]
B --> B4[Prometheus]
B --> B5[Immich]
B --> B6[Home Assistant]

C[Helios Server] -->|Media Stack| C1[Plex]
C --> C2[qBittorrent]
C --> C3[Radarr]
C --> C4[Sonarr]
C --> C5[Lidarr]
C --> C6[Bazarr]
C --> C7[Readarr]
C --> C8[Tautulli]
