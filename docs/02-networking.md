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

# Azure Firewall - Notas de Clase

## Objetivos de Aprendizaje
Al finalizar esta clase, el estudiante será capaz de comprender los fundamentos de Azure Firewall y su diferenciación clave frente a un Network Security Group (NSG). Asimismo, dominará los conceptos detrás de la arquitectura Hub-and-Spoke, los mecanismos de creación de reglas de red, aplicación y NAT, y la implementación práctica del servicio mediante Azure CLI, permitiéndole analizar escenarios avanzados de enrutamiento y seguridad en la nube.

## ¿Qué es Azure Firewall?
Azure Firewall es un servicio de seguridad administrado por Microsoft diseñado específicamente para proteger los recursos desplegados dentro de la plataforma de Azure. Al operar bajo el modelo de Firewall-as-a-Service (FWaaS), la responsabilidad de la infraestructura, la escalabilidad y la alta disponibilidad recae completamente en Microsoft, liberando a los administradores de tareas operativas complejas.

<img width="827" height="562" alt="Esquema Azure Firewall" src="https://github.com/user-attachments/assets/1bf62401-a343-4982-968f-a4c3b435e64f" />

<img width="741" height="515" alt="Arquitectura de Red" src="https://github.com/user-attachments/assets/d1537cc6-ad24-406e-b678-346f99da5c3f" />

<img width="601" height="437" alt="Flujo de Seguridad" src="https://github.com/user-attachments/assets/fc0a68d7-2806-46c1-bd83-10d6b3cba6d9" />

Este servicio opera de manera integral abarcando principalmente las capas 3, 4 y 7 del modelo OSI para garantizar una defensa en profundidad.

| Capa | Función |
| :--- | :--- |
| **L3** | Filtrado por dirección IP |
| **L4** | Filtrado por puertos y protocolos |
| **L7** | Filtrado por aplicaciones y dominios |

## ¿Por qué existe Azure Firewall?
Imagine una organización que cuenta con un ecosistema tecnológico diverso compuesto por aplicaciones web, máquinas virtuales, bases de datos y múltiples servicios internos. Todos estos recursos críticos necesitan una protección robusta y constante contra accesos no autorizados, infecciones por malware, exfiltración de datos sensibles y la comunicación accidental o maliciosa con dominios comprometidos. Azure Firewall surge para solucionar esta necesidad al centralizar por completo el control y las políticas de seguridad perimetral de la red.

## Características Principales

### 1. Stateful Inspection
El servicio cuenta con inspección de estado, lo que significa que el firewall recuerda el contexto de las conexiones activas. Si una máquina virtual inicia una petición hacia Internet, el firewall registra dicha sesión. Cuando la respuesta regresa desde Internet hacia la máquina virtual, el tráfico se permite de forma automática sin necesidad de crear una regla de entrada adicional, ya que el sistema reconoce que pertenece a una comunicación legítima previamente establecida.

### 2. Alta Disponibilidad
La plataforma ofrece alta disponibilidad nativa directamente empaquetada en el servicio. Esto elimina por completo la necesidad de que los ingenieros configuren balanceadores de carga tradicionales, arquitecturas de clustering complejas o mantenimientos manuales costosos, puesto que Microsoft se encarga de garantizar la resiliencia del sistema de fondo.

### 3. Escalabilidad Automática
El cortafuegos está diseñado para ser elástico, lo que le permite aumentar o disminuir sus recursos de cómputo y rendimiento de forma automatizada basándose en la carga de tráfico y las demandas en tiempo real de la organización.

### 4. Threat Intelligence
Incorpora de forma nativa la inteligencia de amenazas de Microsoft, la cual se nutre de una base de datos global constantemente actualizada con direcciones IP sospechosas, dominios maliciosos e indicadores de compromiso conocidos. Gracias a esto, el firewall puede configurarse para alertar o bloquear de manera proactiva cualquier intento de comunicación con entes peligrosos.

### 5. Registro y Monitoreo
La observabilidad está totalmente cubierta mediante una integración nativa con el ecosistema de monitoreo de la nube, permitiendo enviar todos los registros de actividad hacia Azure Monitor, repositorios de Log Analytics y sistemas SIEM avanzados como Microsoft Sentinel.

## Azure Firewall vs Network Security Group (NSG)

Un Network Security Group (NSG) opera a un nivel más básico filtrando el tráfico de red únicamente mediante el análisis de la dirección IP, el puerto y el protocolo, careciendo de la capacidad para inspeccionar el contenido de la capa de aplicación. Por ejemplo, un NSG puede configurarse para permitir el puerto TCP 443, pero no tiene herramientas para discernir si ese tráfico legítimo se dirige hacia un sitio seguro como Google o GitHub, o si está viajando hacia un dominio de distribución de malware.

Por el contrario, Azure Firewall ofrece capacidades avanzadas capaces de inspeccionar a fondo dominios, aplicaciones y URLs completas. Utilizando el mismo escenario del puerto HTTPS 443, Azure Firewall permite establecer políticas granulares para autorizar explícitamente el acceso a herramientas corporativas como GitHub mientras bloquea de manera simultánea dominios maliciosos, a pesar de que ambos flujos utilicen exactamente el mismo puerto y protocolo.

## SKUs de Azure Firewall

El nivel **Basic** está diseñado específicamente para pequeñas empresas, entornos de laboratorio o escenarios de prueba que requieren funciones de protección esenciales sin incurrir en costos elevados.

El nivel **Standard** está pensado para entornos de producción generales e incluye capacidades robustas como filtrado por FQDN, Threat Intelligence integrado, traducciones de red DNAT, así como la gestión tradicional de reglas de red y de aplicación.

El nivel **Premium** representa la máxima protección para entornos corporativos críticos, expandiendo las capacidades del nivel Standard al añadir inspección profunda de tráfico cifrado (TLS Inspection), sistemas de detección y prevención de intrusos (IDS/IPS), filtrado detallado de URL y herramientas de protección avanzada contra amenazas persistentes.

## Arquitectura Hub-and-Spoke
Esta topología representa el modelo de diseño de red recomendado por Microsoft para organizar los recursos en la nube de forma segura y eficiente.

La sección **Hub** actúa como la red central o zona de tránsito, y es el lugar idóneo donde se centralizan los servicios compartidos de seguridad y conectividad, albergando componentes críticos como Azure Firewall, VPN Gateways o circuitos de ExpressRoute.

Las secciones **Spokes** actúan como redes periféricas e independientes conectadas directamente al Hub. Estas zonas están destinadas a contener las cargas de trabajo de la organización, tales como aplicaciones específicas, componentes de bases de datos y máquinas virtuales.

### Flujo de Tráfico
El ciclo de vida de un paquete de datos comienza cuando una máquina virtual ubicada en un Spoke genera tráfico hacia el exterior. Inmediatamente, una ruta definida por el usuario (UDR) intercepta ese tráfico y lo redirige de manera forzada hacia el Hub. Una vez allí, Azure Firewall recibe los paquetes y realiza la inspección correspondiente según las políticas establecidas. Finalmente, el firewall toma la decisión de permitir o bloquear el paquete; si es aprobado, el tráfico continúa su curso hacia el destino final.

## ¿Qué es una UDR?
Las User Defined Routes (UDR) son tablas de enrutamiento personalizadas que permiten a los administradores de red anular las rutas por defecto de Azure para definir manualmente el siguiente salto del tráfico. En un escenario sin UDR, una máquina virtual intentaría comunicarse directamente con Internet de manera desprotegida. Al implementar una UDR con una ruta hacia el destino general `0.0.0.0/0` especificando como siguiente salto (Next Hop) la dirección de Azure Firewall, se garantiza que todo el tráfico saliente sea canalizado obligatoriamente a través del dispositivo de seguridad antes de salir a la red pública.

## Tipos de Reglas

Las **NAT Rules** se utilizan principalmente para publicar servicios internos hacia el exterior, permitiendo que el tráfico proveniente de Internet llegue a la IP pública del firewall y este lo traduzca (DNAT) para redirigirlo de forma segura a un servidor web privado interno.

Las **Network Rules** se enfocan en la seguridad a nivel de red, permitiendo filtrar las comunicaciones mediante la combinación de direcciones IP origen/destino, puertos específicos y protocolos de transporte, como por ejemplo, autorizar que una subred interna acceda al puerto TCP 443 de un destino específico.

Las **Application Rules** ofrecen un filtrado de alta granularidad basado en nombres de dominio completamente calificados (FQDN). Esto permite redactar reglas de negocio claras, como permitir la navegación saliente hacia un dominio de desarrollo y bloquear explícitamente redes sociales u otros sitios no productivos.

## Orden de Evaluación
Para procesar el tráfico entrante y saliente, Azure Firewall sigue un orden estrictamente jerárquico. Primero se evalúan las reglas de tipo NAT; si no hay ninguna coincidencia, se procede a verificar las reglas de Red (Network Rules); por último, se revisan las reglas de Aplicación (Application Rules). Si el tráfico analizado no coincide con ninguna de las reglas explícitamente creadas en los tres niveles anteriores, se aplica un **Deny Implícito**, lo que significa que todo tráfico no autorizado explícitamente es bloqueado por defecto.

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

