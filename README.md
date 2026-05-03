# Intermodular-Planificaci-n-y-administraci-n-de-redes
🏋️ FitLife GYM — Planificación y Administración de Redes

Módulo: Planificación y Administración de Redes
Proyecto: Sistema Informático — FitLife GYM
Curso: 2025–2026


📋 Descripción del proyecto
FitLife GYM es un gimnasio moderno que requiere una infraestructura de red robusta para soportar sus operaciones: gestión de socios, control de acceso, sistemas de pago, videovigilancia y conectividad para empleados y clientes.
Este repositorio contiene toda la documentación técnica del módulo de Planificación y Administración de Redes, incluyendo el análisis de necesidades, diseño de topología, plan de direccionamiento IP, dispositivos y servicios de red.

📁 Estructura del repositorio
fitlife-gym-red/
├── README.md                         ← Este archivo
├── docs/
│   └── FitLife_GYM_Red.docx          ← Documento técnico completo
├── diagrams/
│   └── network_diagram.svg           ← Diagrama de topología de red
└── addressing/
    └── ip_plan.md                    ← Plan de direccionamiento IP (resumen)

🏗️ Arquitectura de red
Topología adoptada
Se utiliza una topología en estrella jerárquica con tres capas:
INTERNET (ISP)
     │
  Router WAN  (IP Pública)
     │
  Firewall UTM  (NAT · VPN · Reglas entre VLANs)
     │
  Switch Core  (24p Gigabit, 802.1Q, SNMP)
   ┌──┬──┬──┐
  SRV NAS SW-PB SW-PA APs
VLANs definidas
VLANNombreRedPropósito10VLAN_ADMIN192.168.10.0/24Servidores y PCs administración20VLAN_RECEPCION192.168.20.0/24Recepción, impresoras, control acceso30VLAN_WIFI_EMP192.168.30.0/24Tablets y dispositivos empleados (WiFi)40VLAN_WIFI_CLI192.168.40.0/24WiFi clientes (solo Internet)50VLAN_CCTV192.168.50.0/24Cámaras IP y grabador NVR (aislada)99VLAN_MGMT192.168.99.0/24Gestión OOB de switches y APs

🌐 Plan de direccionamiento IP (resumen)
Dispositivos clave
DispositivoIPVLANFirewall / Gateway192.168.X.1 (por VLAN)TodasServidor principal (App + BBDD)192.168.10.1010Servidor NAS192.168.10.1110PC Recepción 1192.168.20.1020PC Recepción 2192.168.20.1120PC Admin 1192.168.10.2010PC Admin 2192.168.10.2110Impresoras192.168.20.20-2120Control acceso ×4192.168.20.30-3320Cámaras IP ×8192.168.50.20-2750Switch Core192.168.99.1099APs WiFi ×4192.168.99.20-2399DHCP Clientes WiFi192.168.40.50–20040

🖧 Dispositivos de red
DispositivoModelo de referenciaFunciónRouter WANMikroTik hEX RB750Gr3Conexión ISP, NATFirewall UTMpfSense / FortiGate 40FSeguridad, VPN, VLAN routingSwitch CoreTP-Link TL-SG3428Distribución central, VLANsSwitch acceso ×2TP-Link TL-SG2210P PoEAcceso por plantaAPs WiFi ×4TP-Link EAP245Cobertura WiFi dual-band

⚙️ Servicios de red
ServicioDescripciónDHCPPor VLAN en el Firewall. IPs estáticas para servidores e infraestructuraDNSBind9 interno en servidor principal + forwarding a 8.8.8.8Aplicación webApache/Nginx + MySQL para gestión de socios y reservasNAS / ArchivosSamba/SMB para compartir carpetas entre administraciónVPNWireGuard en Firewall para acceso remoto seguro del administradorCCTV8 cámaras IP + NVR en VLAN 50 aisladaMonitorizaciónZabbix/Grafana + SNMP para supervisión de redControl de acceso4 lectores en VLAN 20, integrados con la aplicación de socios

🔒 Seguridad

Segmentación VLAN: CCTV y WiFi clientes completamente aislados de la LAN interna
Firewall UTM: reglas explícitas entre VLANs (denegación por defecto)
WiFi empleados: WPA3-Enterprise / WiFi clientes: WPA2-PSK con portal cautivo
VPN: acceso remoto con autenticación de doble factor
Gestión OOB: VLAN 99 separada para administrar switches y APs
IPs estáticas: todos los dispositivos críticos con IP fija para trazabilidad


📊 Diagrama de red
El diagrama completo se encuentra en diagrams/network_diagram.svg.
INTERNET
   │
[Router WAN] ─── IP Pública ISP
   │
[Firewall UTM] ─── NAT · VPN · Reglas VLAN
   │
[Switch Core 24p]
 ┌────┬────┬────┬────┐
[SRV][NAS][SW-PB][SW-PA][APs×4]
              │         │      └─ WiFi Empleados (VLAN 30)
              │         │        └─ WiFi Clientes (VLAN 40)
         [Recepción]  [Clases]
         [CCTV ×4]   [CCTV ×4]
         [Control]
         [Impresoras]

📄 Documentación completa
El documento técnico completo con todos los apartados, tablas detalladas y análisis está disponible en:
