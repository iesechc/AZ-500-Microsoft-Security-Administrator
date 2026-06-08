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

Laboratorio Azure CLI
Crear Resource Group
az group create \
--name rg-firewall-demo \
--location eastus
Crear VNet
az network vnet create \
--resource-group rg-firewall-demo \
--name hub-vnet \
--address-prefix 10.0.0.0/16
Crear Subnet AzureFirewallSubnet
az network vnet subnet create \
--resource-group rg-firewall-demo \
--vnet-name hub-vnet \
--name AzureFirewallSubnet \
--address-prefix 10.0.1.0/24
Crear IP Pública
az network public-ip create \
--resource-group rg-firewall-demo \
--name firewall-pip \
--sku Standard
Crear Firewall
az network firewall create \
--resource-group rg-firewall-demo \
--name corp-firewall
Asociar IP Pública
az network firewall ip-config create \
--firewall-name corp-firewall \
--name fw-config \
--public-ip-address firewall-pip \
--resource-group rg-firewall-demo \
--vnet-name hub-vnet
Crear Regla de Aplicación

Permitir acceso a GitHub:

az network firewall application-rule create \
--collection-name AllowGithub \
--firewall-name corp-firewall \
--name GithubRule \
--resource-group rg-firewall-demo \
--action Allow \
--priority 100 \
--protocols Http=80 Https=443 \
--source-addresses "*" \
--target-fqdns github.com
Caso Real Empresarial

Empresa:

50 aplicaciones
10 VNets
500 máquinas virtuales

Problema:

Cada VNet tenía reglas diferentes.

Solución:

Hub-and-Spoke + Azure Firewall.

Beneficios:

Administración centralizada
Auditoría simplificada
Menor superficie de ataque
Cumplimiento normativo
Preguntas de Repaso
¿Cuál es la diferencia principal entre Azure Firewall y NSG?
¿Por qué son necesarias las UDR en una arquitectura Hub-and-Spoke?
¿Cuál es el orden correcto de evaluación de reglas?
¿Qué ventajas ofrece Azure Firewall Premium sobre Standard?
