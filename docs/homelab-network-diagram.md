# Network Diagram & Design

Documento técnico que describe la topología de red del Homelab, plan de direccionamiento, VLANs, reglas de acceso y recomendaciones operativas. Está pensado para ser la referencia única de red y la base para los playbooks de configuración (switch, router, Proxmox, VMs, Pis y APs).

---

## 1. Diagrama de alto nivel (Mermaid)

```mermaid
flowchart TB
  subgraph Core_Network [Core Network]
    Rtr["ISP Router / Edge Router"]
    Swt["Netgear S350 - Smart Managed Pro"]
    AP["TP-Link Archer C80 - AP"]
  end

  subgraph Servers [Server & Storage]
    Atlas["Atlas Core\n(Proxmox)"]
    Helios["Helios Media Server"]
    DAS1["DAS Principal 64TB"]
    DAS2["DAS Backup 4x4TB (planned)"]
  end

  subgraph Security [Security & Monitoring]
    Overwatch["Overwatch RPi4"]
    Sentinel["Sentinel RPi Zero W"]
    PiHole["Pi-hole Cluster (HA)"]
    WG["WireGuard (PiVPN)"]
  end

  subgraph Clients [Clients & IoT]
    Personal["Personal Devices (VLAN5)"]
    Guest["Guest Devices (VLAN4)"]
    IoT["IoT Devices (VLAN3)"]
    Ender["Ender V3 SE (3D Printer)"]
    Aether["Aether Forge (Workstation)"]
    Hermes["Mac Mini M2"]
  end

  Rtr --> Swt
  Swt --> Atlas
  Swt --> Helios
  Swt --> DAS1
  Swt --> DAS2
  Swt --> Overwatch
  Swt --> Sentinel
  Swt --> AP
  AP --> Personal
  AP --> Guest
  Swt --> IoT
  Swt --> Ender
  Swt --> Aether
  Swt --> Hermes
  Overwatch --> PiHole
  Sentinel --> PiHole
  Overwatch --> WG
  Atlas --> DAS1
  Helios --> DAS1
  Atlas --> DAS2

