# Azure Security Notes

## Protección de Datos

### Cifrado en Tránsito
Para garantizar la confidencialidad e integridad de la información mientras viaja por la red, es fundamental implementar protocolos modernos como HTTPS respaldados por TLS 1.2 o versiones superiores. En el ámbito del almacenamiento, se debe habilitar obligatoriamente la opción de transferencia segura (Secure Transfer) en todas las cuentas de Azure Storage. 

Para las tareas de administración remota, el acceso a sistemas operativos Linux se debe realizar exclusivamente mediante SSH, mientras que los entornos Windows deben gestionarse a través de RDP protegido con TLS. Asimismo, para la transferencia de archivos se deben sustituir los esquemas tradicionales por soluciones cifradas como SFTP o FTPS en lugar del protocolo FTP estándar.

### Buenas Prácticas de Cifrado
La premisa fundamental consiste en erradicar por completo el uso de protocolos inseguros o legados dentro de la infraestructura. Como capa de protección adicional y transparente, Azure cifra de forma automática y nativa todo el tráfico de datos que se desplaza entre sus distintos centros de datos, asegurando el perímetro físico de la nube de Microsoft.

---

## Logging y Monitoreo

### Registros Importantes
La visibilidad es un pilar crítico en la estrategia de seguridad. Por ello, es necesario recolectar y centralizar registros clave, entre los que destacan los NSG Flow Logs para el análisis de flujos de red, los registros detallados de Azure Firewall, las auditorías de eventos del Web Application Firewall (WAF) y los logs de consultas de DNS.

### Herramientas de Monitoreo
La plataforma ofrece un ecosistema integrado para la gestión de eventos. Azure Monitor actúa como el recolector base, mientras que los espacios de trabajo de Log Analytics permiten procesar y consultar grandes volúmenes de datos. Para capacidades avanzadas de correlación y respuesta automatizada ante incidentes (SIEM/SOAR), se utiliza Microsoft Sentinel, complementado con Traffic Analytics para obtener una visibilidad geomapeada y detallada del comportamiento de la red.

### Objetivos del Monitoreo
El despliegue de estas herramientas persigue tres metas operativas claras: detectar amenazas potenciales en tiempo real, proporcionar datos forenses suficientes para investigar incidentes de seguridad y generar alertas automatizadas que permitan reaccionar antes de que ocurra un impacto crítico.

---

## Seguridad de Red

### Segmentación de Red
Una arquitectura de red segura se fundamenta en la división lógica del direccionamiento mediante la creación de redes virtuales (VNets) y subredes (Subnets). Es una regla estricta separar completamente los entornos según su propósito operativo, aislando los recursos de Producción, Desarrollo y Administración para evitar movimientos laterales no autorizados.

### Controles de Acceso
El control del tráfico a nivel de subred e interfaz de red se gestiona a través de los Network Security Groups (NSG), los cuales filtran los paquetes basándose en reglas de propiedad básicas como IP, puerto y protocolo. Para simplificar la administración de estas reglas y evitar el mantenimiento complejo de rangos de IPs estáticas, se utilizan los Application Security Groups (ASG), los cuales permiten agrupar máquinas virtuales bajo etiquetas lógicas de seguridad.

### Recomendaciones de Acceso
Toda política de red debe diseñarse bajo la filosofía de Confianza Cero (Zero Trust), aplicando el principio de denegación por defecto ("Deny by default") para todo el tráfico entrante y saliente, y permitiendo flujos de comunicación únicamente mediante excepciones explícitas y justificadas ("Allow by exception").

---

## Acceso Privado

### Azure Private Link
Este servicio permite establecer canales de comunicación privados hacia los servicios PaaS de Azure o servicios de terceros. Al utilizar Private Link, el tráfico se mantiene completamente dentro de la red troncal de Microsoft, evitando que los recursos expongan endpoints públicos a Internet y reduciendo drásticamente la superficie de ataque.

### Buenas Prácticas de Acceso
Se debe evitar a toda costa la asignación de direcciones IP públicas directas a las máquinas virtuales. En su lugar, el diseño seguro exige canalizar el tráfico entrante y saliente a través de balanceadores de carga, puertas de enlace (Gateways) y Endpoints Privados (Private Endpoints).

---

## Firewall y Protección

### Azure Firewall
Este componente ofrece capacidades de filtrado avanzado de tráfico de capa 3 a capa 7, proporcionando a los administradores un control centralizado de las políticas de red y de aplicación a lo largo de múltiples suscripciones y redes virtuales.

### Web Application Firewall (WAF)
El WAF es un servicio especializado en la protección de aplicaciones web y APIs frente a vulnerabilidades comunes, como las descritas en el OWASP Top 10, y ataques automatizados de denegación de servicio. Este firewall se puede implementar de manera integrada en servicios de distribución como Application Gateway, Azure Front Door y Azure CDN.

### Protección DDoS
Azure incluye por defecto el nivel DDoS Basic para proteger la infraestructura global contra ataques masivos de denegación de servicio. Para organizaciones que requieren un nivel de protección personalizado, el nivel DDoS Standard ofrece mitigación avanzada y sintonización de métricas en tiempo real enfocadas en la capa 7 del modelo OSI.

---

## Protocolos Inseguros

### Detectar y Deshabilitar
Es un requisito de cumplimiento identificar y remover de la infraestructura cualquier rastro de protocolos obsoletos que comprometan la seguridad, tales como SSL en todas sus versiones, TLS v1.0 y v1.1, SMBv1, SSHv1 y el esquema de autenticación NTLMv1.

### Herramientas Recomendadas
Para llevar a cabo un proceso de descubrimiento continuo y automatizado de estos protocolos vulnerables dentro del tráfico corporativo, se recomienda configurar directivas y libros de trabajo (Workbooks) específicos dentro de Microsoft Sentinel.

---

## Conectividad Privada

### Opciones de Conexión
Para interconectar las oficinas o centros de datos locales con la nube, o comunicar redes internas de Azure entre sí, se dispone de tres mecanismos principales: Azure VPN para túneles cifrados sobre Internet, ExpressRoute para conexiones privadas dedicadas de alta velocidad que evitan la red pública, y VNet Peering para enlazar redes virtuales dentro de Azure con un rendimiento óptimo.

### Objetivo de la Conectividad
El propósito central de estas tecnologías es asegurar que todo el tráfico operativo e institucional viaje exclusivamente por canales privados y seguros, sin tocar en ningún momento el direccionamiento público de Internet.

---

## Servicios Azure Importantes

La siguiente matriz resume los componentes de seguridad esenciales y su rol dentro de la arquitectura de la nube:

| Servicio | Función Principal |
| :--- | :--- |
| **Azure Firewall** | Sistema de filtrado perimetral administrado de capa 3 a 7. |
| **NSG** | Control de acceso básico por IP, puerto y protocolo a nivel de red. |
| **ASG** | Agrupamiento lógico de recursos para simplificar reglas de seguridad. |
| **Private Link** | Conectividad privada a servicios PaaS evitando exposición pública. |
| **Azure WAF** | Protección especializada para aplicaciones web contra el OWASP Top 10. |
| **DDoS Protection** | Mitigación y defensa ante ataques masivos de denegación de servicio. |
| **Azure Monitor** | Consolidación de métricas y visualización del estado de los recursos. |
| **Microsoft Sentinel** | Plataforma centralizada de inteligencia (SIEM/SOAR) para respuesta a incidentes. |
| **ExpressRoute** | Conexión física y dedicada desde el data center local hacia Azure. |

---

## Buenas Prácticas Generales

Para mantener una postura de seguridad robusta en Azure, es mandatorio forzar el uso de TLS 1.2 o superior en todas las conexiones y centralizar la recolección de logs para auditorías forenses. Asimismo, se deben deshabilitar de forma proactiva todos los protocolos inseguros de la infraestructura y eliminar el aprovisionamiento de IPs públicas que no sean estrictamente necesarias. 

Finalmente, la estrategia de defensa en profundidad debe completarse mediante la implementación conjunta de soluciones WAF y protección DDoS, operando siempre bajo el principio del menor privilegio posible y manteniendo un monitoreo continuo del comportamiento del tráfico de red.

# Controles de Seguridad Generales (MCSB)

- <img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/f73c0744-9017-4966-afba-9be68bac3bec" />

# Evaluación de grupos de Seguridad Azure

<img width="1726" height="911" alt="image" src="https://github.com/user-attachments/assets/a4e085a3-3693-4529-86b9-1e7af9730996" />

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

