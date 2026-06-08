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

# Controles de Seguridad Generales (MCSB)

- <img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f73c0744-9017-4966-afba-9be68bac3bec" />

# Evaluación de grupos de Seguridad Azure

<img width="1726" height="911" alt="image" src="https://github.com/user-attachments/assets/a4e085a3-3693-4529-86b9-1e7af9730996" />


Azure Firewall - Notas de Clase
Objetivos de Aprendizaje

Al finalizar esta clase el estudiante será capaz de:

Comprender qué es Azure Firewall.
Diferenciar Azure Firewall de un NSG (Network Security Group).
Entender la arquitectura Hub-and-Spoke.
Crear reglas de red, aplicación y NAT.
Implementar Azure Firewall mediante Azure CLI.
Analizar escenarios avanzados de enrutamiento y seguridad.
¿Qué es Azure Firewall?

Azure Firewall es un servicio de seguridad administrado por Microsoft que protege recursos desplegados en Azure.

Es un Firewall-as-a-Service (FWaaS), lo que significa que Microsoft administra la infraestructura, escalabilidad y alta disponibilidad.

<img width="827" height="562" alt="image" src="https://github.com/user-attachments/assets/1bf62401-a343-4982-968f-a4c3b435e64f" />

<img width="741" height="515" alt="image" src="https://github.com/user-attachments/assets/d1537cc6-ad24-406e-b678-346f99da5c3f" />

<img width="601" height="437" alt="image" src="https://github.com/user-attachments/assets/fc0a68d7-2806-46c1-bd83-10d6b3cba6d9" />



Azure Firewall opera principalmente en las capas 3, 4 y 7 del modelo OSI:

Capa	Función
L3	Filtrado por IP
L4	Filtrado por puertos y protocolos
L7	Filtrado por aplicaciones y dominios
¿Por qué existe Azure Firewall?

Supongamos que una organización tiene:

Aplicaciones Web
Máquinas Virtuales
Bases de Datos
Servicios internos

Todos estos recursos necesitan protección contra:

Accesos no autorizados
Malware
Exfiltración de datos
Comunicación con dominios maliciosos

Azure Firewall centraliza el control de seguridad.

Características Principales
1. Stateful Inspection

Azure Firewall recuerda el estado de las conexiones.

Ejemplo:

VM → Internet

Cuando la respuesta regresa:

Internet → VM

No es necesario crear una regla adicional porque el firewall conoce el estado de la sesión.

2. Alta Disponibilidad

No requiere:

Balanceadores
Clustering
Configuración manual

Microsoft administra automáticamente la disponibilidad.

3. Escalabilidad Automática

El servicio aumenta o disminuye recursos según la carga.

4. Threat Intelligence

Microsoft mantiene una base de datos global de:

IPs maliciosas
Dominios maliciosos
Indicadores de compromiso

El firewall puede:

Alertar
Bloquear

automáticamente.

5. Registro y Monitoreo

Integración nativa con:

Azure Monitor
Log Analytics
Microsoft Sentinel
Azure Firewall vs NSG
NSG

Filtra tráfico usando:

IP
Puerto
Protocolo

No inspecciona aplicaciones.

Ejemplo:

Permitir TCP 443

No sabe si el tráfico va hacia:

google.com
github.com
malware.com
Azure Firewall

Puede inspeccionar:

Dominios
Aplicaciones
URLs

Ejemplo:

Permitir:

github.com

Bloquear:

malware-domain.com

aunque ambos utilicen HTTPS 443.

SKUs de Azure Firewall
Basic

Pensado para:

Pequeñas empresas
Laboratorios
Entornos de prueba

Funciones básicas.

Standard

Incluye:

FQDN Filtering
Threat Intelligence
DNAT
Network Rules
Application Rules
Premium

Incluye todo lo anterior más:

TLS Inspection
IDS/IPS
URL Filtering
Protección avanzada
Arquitectura Hub-and-Spoke

Modelo recomendado por Microsoft.

Hub

Contiene:

Azure Firewall
VPN Gateway
ExpressRoute
Spokes

Contienen:

Aplicaciones
Máquinas virtuales
Servicios
Flujo de Tráfico

Paso 1

VM en Spoke genera tráfico.

↓

Paso 2

UDR redirige tráfico.

↓

Paso 3

Azure Firewall inspecciona.

↓

Paso 4

Firewall decide:

Permitir
Bloquear

↓

Paso 5

Tráfico continúa.

¿Qué es una UDR?

UDR = User Defined Route

Permite definir manualmente hacia dónde debe enviarse el tráfico.

Ejemplo:

Destino:
0.0.0.0/0

Next Hop:
Azure Firewall

Sin UDR:

VM → Internet

Con UDR:

VM → Azure Firewall → Internet

Tipos de Reglas
1. NAT Rules

Utilizadas para publicar servicios internos.

Ejemplo:

Internet → Firewall Public IP → Servidor Web

2. Network Rules

Filtran por:

IP
Puerto
Protocolo

Ejemplo:

Permitir:

10.0.1.0/24 → TCP 443

3. Application Rules

Filtran por:

FQDN
Dominio

Ejemplo:

Permitir:

github.com

Bloquear:

facebook.com

Orden de Evaluación

Azure Firewall evalúa:

NAT Rules
Network Rules
Application Rules

Si ninguna coincide:

DENY IMPLÍCITO

Todo tráfico es bloqueado.

¿Cuál es la diferencia principal entre Azure Firewall y NSG?
¿Por qué son necesarias las UDR en una arquitectura Hub-and-Spoke?
¿Cuál es el orden correcto de evaluación de reglas?
¿Qué ventajas ofrece Azure Firewall Premium sobre Standard?
