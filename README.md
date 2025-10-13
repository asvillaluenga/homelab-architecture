# Homelab Architecture

Documentación técnica de mi infraestructura doméstica autogestionada.  
El objetivo de este proyecto es demostrar diseño de red, segmentación, monitorización y despliegue de servicios en entornos híbridos.

---

## Infraestructura General

- **2× Raspberry Pi** → Pi-hole, PiVPN, Unbound, Recursive DNS, Webmin  
- **Dell Latitude 7390 #1 (Ubuntu Server)** → NetAlertX, Uptime Kuma, MySpeed, Tautulli, Overseerr  
- **Dell Latitude 7390 #2 (Windows 11)** → Plex, Suite ARR, qBittorrent, Docker, Portainer  
- **Mac Mini M2** → pruebas y virtualización ligera  
- **Windows Server 2022 y Kali Linux** → entornos de laboratorio y simulación de amenazas

---

## Topología de Red

![Network Diagram](./docs/homelab-network-diagram.png)

- Red principal segmentada en subredes (IoT, LAN, Media, VPN)
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
| OS | Ubuntu Server, Windows Server, Kali Linux |
| Hardware | Raspberry Pi, Dell Latitude, Mac Mini |

---

 **Documentación complementaria:**  
- [Hardware Inventory](./docs/hardware-inventory.md)  
- [Docker Stack Overview](./docs/docker-stack-overview.png)

> _"Si no lo puedes medir, no lo puedes mejorar."_
