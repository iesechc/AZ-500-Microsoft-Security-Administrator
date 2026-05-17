# Azure Security Notes

## Protección de Datos

### Cifrado en tránsito
- Usar HTTPS y TLS 1.2 o superior.
- Habilitar **Secure Transfer** en Azure Storage.
- Administración remota segura:
  - Linux → SSH
  - Windows → RDP/TLS
- Usar SFTP/FTPS en lugar de FTP.

### Buenas prácticas
- Evitar protocolos inseguros.
- Azure ya cifra tráfico entre datacenters automáticamente.

---

# Logging y Monitoreo

## Registros importantes
- NSG Flow Logs
- Azure Firewall Logs
- WAF Logs
- DNS Logs

## Herramientas
- Azure Monitor
- Log Analytics
- Microsoft Sentinel
- Traffic Analytics

## Objetivo
- Detectar amenazas.
- Investigar incidentes.
- Generar alertas de seguridad.

---

# Seguridad de Red

## Segmentación de red
- Crear VNets y Subnets.
- Separar ambientes:
  - Producción
  - Desarrollo
  - Administración

## Controles de acceso
- NSG → Filtrado por IP, puerto y protocolo.
- ASG → Agrupar reglas de seguridad.

## Recomendaciones
- Aplicar:
  - “Deny by default”
  - “Allow by exception”

---

# Acceso Privado

## Azure Private Link
- Acceso privado a servicios Azure.
- Evita exposición a Internet.

## Buenas prácticas
- Evitar IPs públicas directas en VMs.
- Usar:
  - Load Balancers
  - Gateways
  - Private Endpoints

---

# Firewall y Protección

## Azure Firewall
- Filtrado avanzado de tráfico.
- Control centralizado de red.

## WAF (Web Application Firewall)
Servicios compatibles:
- Application Gateway
- Front Door
- Azure CDN

Protege contra:
- OWASP Top 10
- Ataques web y APIs

## Protección DDoS
- DDoS Basic → incluido por defecto.
- DDoS Standard → protección avanzada Layer 7.

---

# Protocolos Inseguros

## Detectar y deshabilitar
- SSL/TLS v1
- SMBv1
- SSHv1
- NTLMv1

## Herramienta recomendada
- Microsoft Sentinel

---

# Conectividad Privada

## Opciones
- Azure VPN
- ExpressRoute
- VNet Peering

## Objetivo
- Mantener tráfico privado dentro de Azure.

---

# Servicios Azure Importantes

| Servicio | Función |
|---|---|
| Azure Firewall | Firewall administrado |
| NSG | Filtrado de red |
| ASG | Agrupación de seguridad |
| Private Link | Acceso privado |
| Azure WAF | Protección web |
| DDoS Protection | Mitigación DDoS |
| Azure Monitor | Monitoreo |
| Microsoft Sentinel | SIEM/SOAR |
| ExpressRoute | Conexión privada |

---

# Buenas Prácticas Generales

- Usar TLS 1.2+
- Centralizar logs
- Deshabilitar protocolos inseguros
- Evitar IPs públicas innecesarias
- Implementar WAF + DDoS
- Aplicar mínimo privilegio
- Monitorear tráfico continuamente

- <img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f73c0744-9017-4966-afba-9be68bac3bec" />

