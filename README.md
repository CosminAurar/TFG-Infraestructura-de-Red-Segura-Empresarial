# 🏗️ TFG-Infraestructura-de-Red-Segura-Empresarial
Ciclo Formativo: Administración de Sistemas Informáticos en Red (ASIR)
Autores: Juan David Sandoval · Jorge Sánchez · Cosmin Aurar
Curso: 2025 – 2026 · Madrid

# 📋 Descripción
Diseño e implementación de una infraestructura de red segura completa para la empresa ficticia TechSolutions SL, una consultora IT con 75 empleados. Todo el entorno fue desplegado en VirtualBox sin hardware físico dedicado.

# 🧱 Arquitectura
ZonaRango IPFunciónWAN192.168.1.0/24Conexión exteriorDMZ172.16.10.0/24Servidor web corporativoVLAN_Admin10.10.30.0/24Administración de sistemasVLAN_Dev10.10.40.0/24Equipo de desarrolloVLAN_MGMT10.10.99.0/24Monitorización (Wazuh)

# ⚙️ Tecnologías implementadas

pfSense CE 2.8.1 — Doble firewall (externo + interno) con zona DMZ
Suricata — IDS/IPS con reglas personalizadas en ambos firewalls
Samba AD — Directorio activo corporativo (Ubuntu Server 24.04)
Wazuh 4.14 — SIEM con agentes en Linux y Windows
YARA + VirusTotal API — Detección de malware en endpoints
Apache2 — Servidor web corporativo en la DMZ
VirtualBox — Entorno de virtualización completo
