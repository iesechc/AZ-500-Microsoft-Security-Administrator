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

**¿Cómo puedo conectar mis aplicaciones a servicios de Azure de forma privada y segura?**

Azure ofrece tres herramientas principales para esto:
1. **Service Endpoints** – la opción más sencilla
2. **Private Endpoints** – mayor seguridad y flexibilidad
3. **Private Link Services** – para exponer tus propios servicios de forma privada


**¿Qué es un Service Endpoint?]**

Imaginen que tienen una cuenta de Azure Storage con datos confidenciales. Por defecto, esa cuenta tiene una IP pública. Cualquiera que conozca esa IP —al menos en teoría— podría intentar acceder.

Un **Service Endpoint** extiende la identidad de su red virtual hasta el servicio de Azure. El tráfico nunca sale a internet: viaja directamente por la red troncal (backbone) de Microsoft.

Service Endpoints está disponible para una lista importante de servicios:
- Azure Storage, Azure SQL Database, Cosmos DB, Key Vault
- Service Bus, Event Hubs, App Service, y más

Todos en disponibilidad general en todas las regiones de Azure.

Limitaciones clave:
- Solo funciona con el modelo Azure Resource Manager (no Classic)
- Para Azure SQL: solo aplica en la misma región
- No sirve para tráfico desde redes on-premises directamente
- Máximo 200 combinaciones de suscripción/región por servicio

**¿Qué es un Private Endpoint?]**

Microsoft recomienda usar **Private Endpoints** sobre Service Endpoints para mayor seguridad.

Un Private Endpoint es una **interfaz de red** dentro de su VNet que obtiene una IP privada. Esta interfaz se conecta directamente al servicio de Azure usando Azure Private Link.

**Diferencia clave con Service Endpoints:** Con un Service Endpoint, el servicio sigue teniendo su IP pública; solo cambias la ruta. Con un Private Endpoint, el servicio obtiene una IP privada dentro de TU red. Es como traer el servicio dentro de tu VNet.

Cuando crean un Private Endpoint definen:
- **Nombre** único en el grupo de recursos
- **Subred** donde se asignará la IP privada
- **Recurso de Private Link** al que se conectará
- **Método de aprobación:** automático o manual

Los estados de conexión son: Pending → Approved → Rejected o Disconnected.


**Consideraciones de red]**

- La interfaz de red se crea automáticamente y es de solo lectura
- La IP privada asignada NO cambia durante el ciclo de vida del endpoint
- Soporta NSG (Network Security Groups), UDR (User Defined Routes) y ASG (Application Security Groups)
- El Private Endpoint debe estar en la misma región y suscripción que la VNet

**Regla de oro de DNS:** El DNS debe resolver el FQDN del servicio a la IP privada del endpoint. Si el DNS resuelve a la IP pública, el tráfico no pasará por el Private Endpoint. Usen Azure Private DNS Zones para esto.

**¿Qué es Private Link Service?]**

Hasta ahora hablamos de conectarse a servicios de Microsoft. Pero, ¿qué pasa si **ustedes son el proveedor** y quieren exponer su propio servicio de forma privada a sus clientes?
l flujo es:
1. Su servicio corre detrás de un **Standard Load Balancer**
2. Crean el **Private Link Service** apuntando al Load Balancer
3. Azure genera un **alias** único (Ej: `miservicio.{GUID}.eastus.azure.privatelinkservice`)
4. Comparten el alias con sus clientes
5. Los clientes crean un **Private Endpoint** usando ese alias

Control de visibilidad y acceso]**

Tienen tres opciones de visibilidad:
- **Solo RBAC:** Para consumo interno entre VNets del mismo tenant
- **Por suscripción:** Restringen acceso a suscripciones de confianza
- **Cualquier persona con el alias:** Acceso público (requieren aprobación manual)

Y dos modos de aprobación:
- **Auto-approval:** Las suscripciones en la lista se aprueban automáticamente
- **Manual:** El dueño del servicio aprueba cada solicitud

**Limitaciones importantes:**
- Solo funciona con Standard Load Balancer (no Basic)
- Solo tráfico IPv4
- Solo TCP y UDP
- Timeout de ~5 minutos en idle


VNet Integration en App Service]**

Las aplicaciones en App Service viven en workers compartidos de Azure. Con **VNet Integration**, montamos interfaces virtuales en esos workers para que el tráfico saliente pueda llegar a recursos privados.

> ⚠️ **Distinción crucial:** VNet Integration controla el tráfico **saliente** de la app. Para controlar tráfico **entrante** (quién puede llamar a la app), usan Private Endpoints o Access Restrictions.

**Requisitos**

- Tier: Basic, Standard, Premium v2/v3, o Elastic Premium
- Requiere una **subred dedicada** delegada a App Service
- Recomendación: `/26` (64 direcciones) para tener margen de escala
- La app y la VNet deben estar en la **misma región**

**Cálculo de IPs:**
- 5 IPs reservadas por Azure en cualquier subred
- 1 IP por instancia del App Service Plan
- Al escalar, se duplican temporalmente las IPs necesarias

**Tipos de routing]**

Tres tipos de routing configurables:
1. **Application Routing:** Qué tráfico de la app va por la VNet (todo o solo RFC1918/privado)
2. **Configuration Routing:** Tráfico durante el arranque (container pull, Key Vault references)
3. **Network Routing:** NSGs y UDRs aplicados a la subred de integración

** Si su app usa referencias a Key Vault y el vault bloquea tráfico público, asegúrense de que el routing de configuración esté habilitado para que las secrets se resuelvan por la VNet.

**¿Cuándo usar cada uno?]**

Guía de decisión rápida:
- **Service Endpoint** → Necesitan ruta privada simple y rápida; sin acceso desde on-premises; sin costo extra
- **Private Endpoint** → Máxima seguridad; necesitan acceso desde on-premises o entre regiones; OK con costo adicional y configurar DNS
- **Private Link Service** → Son el proveedor; quieren exponer su servicio privadamente a clientes externos
- **VNet Integration** → Su app en App Service necesita alcanzar recursos privados en una VNet

