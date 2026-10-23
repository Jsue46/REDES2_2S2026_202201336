# Proyecto 2 — Redes de Computadoras 2

**Universidad de San Carlos de Guatemala (USAC)**  
**Facultad de Ingeniería — Escuela de Ciencias y Sistemas**  
**Segundo Semestre 2026**  

---

## Datos del Estudiante
- **Nombre:** Estuardo Josué Vaquiax Gómez
- **Carné:** `202201336`
- **Variables Asignadas:** $P = 2$, $U = 6$, $XX = 36$

---

## Proyecto Conectividad XXI: Integración de Operadores Nacionales

Este repositorio contiene la entrega oficial del **Proyecto 2**, enfocado en la interconexión de tres Proveedores de Servicios de Internet (ISPs) nacionales mediante el protocolo de enrutamiento exterior **BGP** sobre enlaces punto a punto de fibra óptica, implementando alta disponibilidad, servicios centralizados y segmentación de seguridad.

---

## Estructura del Repositorio

```text
├── Proyecto2_202201336.pkt              # Archivo de simulación en Cisco Packet Tracer 8.2+
├── Manual_Tecnico.md                    # Manual técnico exhaustivo (VLSM, CLI, ACLs, evidencias)
├── Proyecto2_Presentacion_202201336.pdf  # Presentación ejecutiva para stakeholders (costos y arquitectura)
├── Wireshark/
│   └── captura_wireshark_202201336.pcapng # Captura de tráfico controlado (TCP, DNS, TLS, ICMP)
├── image.png a image-7.png              # Evidencias fotográficas de pruebas y conmutación
├── 975_Proyecto_2_2S2026.pdf            # Enunciado y rúbrica oficial del proyecto
└── README.md                            # Documento de bienvenida y descripción general
```

---

## Resumen de la Arquitectura Implementada

### 1. Núcleo BGP e Interconexión de ISPs
- **ISP 1 (FibraNet GT - AS 65001):** Enrutamiento interno OSPF Área 0 (`172.16.16.0/24`). Aloja el Servidor Web HTTP/HTTPS (`172.16.16.10`) y Servidor DNS (`172.16.16.11`) resolviendo `www.proyecto2_202201336.com`.
- **ISP 2 (AndinaCom - AS 65002):** Enrutamiento interno EIGRP AS 200 (`172.16.26.0/24`). Aloja el Servidor DHCP Central (`172.16.26.130`) y redundancia de primer salto **HSRP** (IP virtual `172.16.26.3`).
- **ISP 3 (MoonLink - AS 65003):** Enrutamiento estático hacia el Core BGP (`172.16.32.0/24`). Integra un Router Inalámbrico Wi-Fi (SSID: `MoonLink_WiFi`) con WPA2-PSK y DHCP para hosts móviles.

### 2. Redundancia de Enlace y Puerta de Enlace
- **LACP (IEEE 802.3ad):** Enlaces redundantes agrupados en EtherChannel (Canales 1 y 2 en cada ISP).
- **HSRP (Cisco Hot Standby Router Protocol):** R2_Master (`172.16.26.1`, Prioridad 110, Preempt) y R2_Backup (`172.16.26.2`, Prioridad 90) garantizan cero pérdida de servicio.

### 3. Matriz de Seguridad y Políticas de Acceso (ACLs)
- **ISP 1:** Deniega tráfico HTTP (puerto 80) proveniente de ISP 2, forzando el uso exclusivo de HTTPS (puerto 443).
- **ISP 2:** Deniega acceso directo al Servidor DHCP desde el departamento de Ventas.
- **ISP 3:** Aislamiento total del departamento de Seguridad (únicamente tráfico de respuesta establecido permitido).

### 4. Análisis Forense de Red (Wireshark)
- Inspección paquete a paquete en [`Wireshark/captura_wireshark_202201336.pcapng`](Wireshark/captura_wireshark_202201336.pcapng):
  - **Escenario 1:** Establecimiento de sesión TCP (Handshake SYN, SYN-ACK, ACK).
  - **Escenario 2:** Resolución de nombres DNS vía UDP puerto 53.
  - **Escenario 3:** Contraste de texto plano HTTP vs cifrado TLS 1.3 / HTTPS.
  - **Escenario 4:** Diagnóstico de latencia y alcance con ICMP Echo Request / Reply.

---

## Instrucciones para Evaluación

1. **Abrir la simulación:** Utilizar Cisco Packet Tracer versión 8.2 o superior para abrir [`Proyecto2_202201336.pkt`](Proyecto2_202201336.pkt).
2. **Consultar la documentación técnica:** Revisar [`Manual_Tecnico.md`](Manual_Tecnico.md) para detalles de comandos CLI, tabla de direccionamiento y pruebas paso a paso.
3. **Presentación ejecutiva:** Visualizar [`Proyecto2_Presentacion_202201336.pdf`](Proyecto2_Presentacion_202201336.pdf) para el análisis de costos y propuesta para stakeholders.
