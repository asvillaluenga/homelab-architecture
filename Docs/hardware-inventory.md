# Hardware Inventory

Inventario completo del hardware que forma la infraestructura del Homelab.  
Este documento detalla cada nodo, su función, recursos disponibles y cómo se integran en la arquitectura general de seguridad, virtualización, almacenamiento, automatización y servicios multimedia.

---

## 1. Compute Nodes

### 1.1 Atlas Core  
**Intel i7-9850H | 64GB RAM | NVMe 1TB Gen4**  
Servidor principal de virtualización corriendo **Proxmox VE**, responsable de alojar las cargas críticas del homelab.

**Rol:** Core Hypervisor & Orchestration Layer  

---

### 1.2 Helios Media Server  
**Intel i5-8365U | 16GB RAM | NVMe 1TB + DAS 64TB**  

Servidor dedicado a **media, automatización y content indexing**.

**Rol:** Media & Automation Node  

---

### 1.3 Overwatch Node (Raspberry Pi 4)  
**Raspberry Pi 4 – 4GB | SSD 120GB**  
Nodo ARM principal, siempre encendido, responsable de la capa de seguridad, automatización ligera y monitorización.

**Rol:** Security & Monitoring Gateway Node  

---

### 1.4 Sentinel HA Node (Raspberry Pi Zero W)  
**Raspberry Pi Zero W**  

Nodo secundario para DNS redundante y alta disponibilidad.

**Rol:** High Availability DNS & Ad-Blocking  

---

### 1.5 Hermes Node (Mac Mini M2)  
**16GB RAM**  
Nodo destinado a investigación, desarrollo y pruebas con modelos LLM.

**Rol:** AI Research Node  

---

### 1.6 Aether Forge (Ryzen 9 3900X + RTX 3070)  
**Ryzen 9 3900X | GPU RTX 3070**  

**Rol:** Generative AI Workstation  

---

## 2. Storage Systems

### 2.1 DAS Principal — 4 Bahías (Operativo)
**Capacidad total: 64TB (antes de redundancia o particionado)**

**Configuración de Discos:**
- **2 × 8TB HDD**
- **2 × 16TB HDD**

**Rol:**  
- Almacenamiento primario del homelab  
- Multimedia  
- Backend para Helios y Atlas según servicio

---

### 2.2 HDD Externo – 3TB  
**Rol:**  
- Backups puntuales  
- Almacenamiento frío  
- Archivos no críticos

---

### 2.3 DAS Secundario (Plan) — 4 Bahías  
**Plan de compra:** 4× HDD de 4TB

**Objetivo:**  
- Servidor dedicado de **Backups**  
- Migración de **Immich** a este almacenamiento para aumentar resiliencia  

**Recomendación técnica del RAID (para backup):**  
- **RAID 6:** equilibrio óptimo para un backup server (tolerancia a 2 fallos + buena relación capacidad/seguridad).

---

## 3. Networking Hardware

### 3.1 Netgear Smart Managed Pro S350  
Switch administrable que soporta VLANs, trunking, QoS y ACLs.  
Permite segmentación avanzada de red a nivel empresarial.

**VLANs implementadas:**

| VLAN | Nombre | Descripción |
|------|--------|-------------|
| **1** | ADMIN | Gestión de hardware, switches, routers |
| **2** | SERVERS | Atlas Core, Helios, nodos críticos |
| **3** | IOT | Dispositivos IoT y Pis secundarios |
| **4** | GUEST | Red aislada para visitantes |
| **5** | PERSONAL | PCs, móviles y dispositivos personales |

**Estado:** Totalmente operativa — backbone del homelab.

---

### 3.2 TP-Link Archer C80  
Acces Point configurado para:

- **Red Principal:** VLAN5  
- **Red Guest:** VLAN4  
- **Modo AP puro (sin NAT)**  
- **Compatibilidad WiFi 5 (AC1900)**

**Rol:** Wireless Access Layer

---

## 4. Auxiliary Hardware

### 4.1 Ender V3 SE  
Impresora 3D para prototipado, soportes, brackets, cajas de sensores, racks y diseño de piezas funcionales para la infraestructura del homelab.

**Rol:** Prototipado y fabricación de accesorios para el lab.

---

### 4.2 Raspberry Pi Pico  
Microcontrolador ARM para automatizaciones específicas.

**Aplicaciones previstas:**  
- Telemetría ambiental del rack  
- Supervisión eléctrica DIY  
- Sensores MQTT con Node-RED  
- Paneles de estado o alertas  
- Relés, ventilación, triggers físicos  

---

## 5. Hardware Summary Table

| Componente | Especificaciones | Rol |
|-----------|------------------|-----|
| **Atlas Core** | i7-9850H, 64GB, NVMe 1TB | Virtualización + Orquestación |
| **Helios Server** | i5-8365U, 16GB, NVMe 1TB + DAS 64TB | Media Server |
| **Overwatch Node** | RPi4, 4GB, SSD 120GB | Seguridad, DNS, VPN, Monitorización |
| **Sentinel HA** | RPi Zero W | DNS redundante |
| **Mac Mini M2** | 16GB | AI Research |
| **Aether Forge** | R9 3900X + RTX 3070 | Generative AI |
| **DAS Principal** | 2×8TB + 2×16TB | Almacenamiento masivo |
| **HDD Externo** | 3TB | Backups básicos |
| **DAS Secundario (Plan)** | 4×4TB (RAID6) | Backup avanzado / Immich |
| **Netgear S350** | Smart Managed Switch | Segmentación VLAN |
| **TP-Link Archer C80** | AP WiFi 5 | Wireless Main + Guests |
| **Ender V3 SE** | FDM 3D Printer | Prototipado |
| **RPi Pico** | MCU ARM | Automatización y sensores |

---

## 7. Roadmap de Expansión

1. Adquirir DAS secundario (4×4TB).  
2. Configurar RAID6 para backup e Immich.  
3. Integrar backup incremental y rotación segura entre ambos DAS.  
4. Añadir sensores con RPi Pico.  
5. Documentar pipelines de recuperación ante fallos.  

---
