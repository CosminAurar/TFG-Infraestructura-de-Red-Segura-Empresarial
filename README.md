# 🏗️ Infraestructura de Red Segura Empresarial — TFG ASIR

# 📋 Descripción
Diseño e implementación completa de una infraestructura de red segura para la empresa ficticia TechSolutions SL, una consultora IT con 75 empleados. El entorno fue desplegado íntegramente en VirtualBox, demostrando la viabilidad de construir una arquitectura de seguridad empresarial robusta sin hardware físico dedicado.


# ⚠️ Aviso Legal y Ético

DECLARACIÓN DE USO ÉTICO: Este proyecto se desarrolló en un entorno virtualizado controlado con fines exclusivamente educativos. Las pruebas de ataque realizadas desde Kali Linux se ejecutaron únicamente contra sistemas propios del laboratorio, nunca contra infraestructuras reales o de terceros.

# 🎯 Objetivos del Proyecto


-Implementar una arquitectura de doble firewall con zona DMZ

-Segmentar la red interna mediante VLANs departamentales

-Desplegar un directorio activo corporativo con Samba AD

-Configurar un sistema IDS/IPS con reglas personalizadas

-Implementar un SIEM centralizado con detección de malware

-Verificar la seguridad mediante pruebas de ataque controladas


# 🛠️ Stack Tecnológico

| Componente           | Tecnología            | Versión              |
| -------------------- | --------------------- | -------------------- |
| Firewall             | pfSense CE            | 2.8.1                |
| IDS/IPS              | Suricata              | Integrado en pfSense |
| Directorio Activo    | Samba AD              | Ubuntu Server 24.04  |
| SIEM                 | Wazuh                 | 4.14                 |
| Detección de malware | YARA + VirusTotal API | —                    |
| Servidor web         | Apache2               | —                    |
| Virtualización       | VirtualBox            | —                    |


# 🧱 Arquitectura de Red

| Zona       | Rango IP       | Dispositivos             | Función                              |
| ---------- | -------------- | ------------------------ | ------------------------------------ |
| WAN        | 192.168.1.0/24 | FW-Externo, Kali Linux   | Conexión exterior / atacante         |
| DMZ        | 172.16.10.0/24 | Servidor web             | Servicios expuestos al exterior      |
| TRUNK_FW   | 10.10.100.0/30 | FW-Externo ↔ FW-Interno  | Enlace punto a punto entre firewalls |
| VLAN_Admin | 10.10.30.0/24  | Samba AD, Ubuntu Desktop | Administración de sistemas           |
| VLAN_Dev   | 10.10.40.0/24  | Windows Cliente          | Equipo de desarrollo                 |
| VLAN_MGMT  | 10.10.99.0/24  | Wazuh Server             | Monitorización y gestión             |


# 🔒 Reglas de Firewall — Política de Mínimo Privilegio

| Zona origen | Zona destino | Acceso       | Justificación                   |
| ----------- | ------------ | ------------ | ------------------------------- |
| WAN         | DMZ          | HTTP / HTTPS | Acceso público al servidor web  |
| DMZ         | Red interna  | ❌ Bloqueado  | Aislamiento total de la DMZ     |
| VLAN_Admin  | Todas        | ✅ Completo   | Administración centralizada     |
| VLAN_Dev    | VLAN_MGMT    | ❌ Bloqueado  | Protección del SIEM             |
| VLAN_Dev    | VLAN_Admin   | ✅ Permitido  | Acceso a recursos de desarrollo |


# 🚨 Reglas Personalizadas de Suricata

| SID     | Amenaza detectada           | Técnica                   |
| ------- | --------------------------- | ------------------------- |
| 1000001 | XSS                         | Patrón `<script>` en HTTP |
| 1000002 | Command Injection           | `;whoami` en HTTP         |
| 1000003 | Escaneo Nmap (SYN scan)     | >20 SYN en 5 segundos     |
| 1000010 | Ping sweep / reconocimiento | >5 ICMP en 10 segundos    |
| 1000014 | LFI / Path Traversal        | `/etc/passwd` en HTTP     |
| 1000017 | Reverse shell               | `bash -i` en TCP          |
| 1000020 | Netcat shell                | `nc -e` en TCP            |


# 📊 Resultados de las Pruebas de Seguridad

| Prueba              | Herramienta     | Resultado                | Tiempo de bloqueo |
| ------------------- | --------------- | ------------------------ | ----------------- |
| Escaneo de puertos  | Nmap (SYN scan) | ✅ Bloqueado por Suricata | < 5 segundos      |
| Ping masivo         | Kali Linux      | ✅ Bloqueado por Suricata | < 10 segundos     |
| Acceso DMZ → LAN    | Manual          | ✅ Bloqueado por pfSense  | Inmediato         |
| Detección malware   | YARA            | ✅ Detectado y alertado   | Tiempo real       |
| Análisis VirusTotal | API VirusTotal  | ✅ Hash consultado        | Tiempo real       |

# 📁 Documentación

📄 Memoria TFG Documentación técnica completa: diseño, implementación y justificación

📎 Anexo Configuraciones detalladas, capturas de pantalla y evidencias

# 💡 Decisiones de Diseño y Aprendizajes

# ¿Por qué doble firewall en lugar de uno?

Un único firewall creando la DMZ con tres interfaces es la solución más común y económica, pero presenta un punto único de fallo: si el firewall se ve comprometido, el atacante tiene acceso directo tanto a la DMZ como a la red interna. La arquitectura de doble firewall añade una capa adicional: aunque el FW-Externo sea vulnerado, el FW-Interno actúa como segunda barrera antes de llegar a los sistemas críticos.

# ¿Por qué Wazuh como SIEM?

Wazuh es una plataforma open source con capacidades comparables a soluciones enterprise (Splunk, IBM QRadar) sin coste de licencia. La integración nativa con YARA y la API de VirusTotal permite detección de malware en tiempo real sin infraestructura adicional. Para un entorno con 75 usuarios es una elección realista y escalable.

# ¿Qué haría diferente en producción real?

-Añadiría autenticación multifactor (MFA) para el acceso a VLAN_Admin

-Implementaría logs centralizados con retención mínima de 90 días para cumplimiento normativo (ENS, ISO 27001)

-Separaría la VLAN de gestión de Wazuh en una red out-of-band dedicada

-Consideraría pfSense en alta disponibilidad (HA) con CARP para eliminar el punto único de fallo


