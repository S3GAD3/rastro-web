# 🕵️ RASTRO-WEB

**RASTRO-WEB** es una herramienta web de apoyo a investigaciones **OSINT
(Open Source Intelligence)** centrada en el análisis y correlación de
**dominios, direcciones IP, URLs e infraestructura web relacionada**.

Está diseñada como un **workbench de investigación**: a partir de un
indicador inicial, organiza fuentes OSINT, facilita pivotes, permite
registrar y contrastar hallazgos y construye un grafo de relaciones
entre los distintos elementos descubiertos.

Todo funciona desde un único archivo HTML y **sin necesidad de instalar
software adicional ni disponer de un backend propio**.

> **RASTRO-WEB no es un escáner ni una herramienta de explotación.**
> Está orientada a la investigación pasiva, la consulta de fuentes
> abiertas y la organización de información obtenida legítimamente
> durante una investigación.
>
> **Enlace a la herramienta: https://s3gad3.github.io/rastro-web/

------------------------------------------------------------------------

## 🎯 ¿Qué hace RASTRO-WEB?

La investigación puede comenzar con un único dato:

``` text
example.com
185.199.108.153
https://example.com/login?id=1234
```

RASTRO-WEB detecta automáticamente si el indicador es un **dominio, una
IP o una URL** y adapta las fuentes y pivotes disponibles.

``` text
DOMINIO
   │
   ├── IP
   │    └── ASN
   │         └── PROVEEDOR
   ├── SUBDOMINIOS
   ├── NS / MX
   ├── CERTIFICADOS
   ├── EMAIL
   └── WHOIS HISTÓRICO
        ├── ANTIGUO TITULAR
        ├── EMPRESA
        └── EMAIL HISTÓRICO
```

Los hallazgos pueden conservarse, relacionarse y reutilizarse como
nuevos puntos de investigación.

------------------------------------------------------------------------

## 🔎 Funciones principales

### 🌐 Investigación de dominios

-   DNS actual e histórico
-   WHOIS / RDAP
-   WHOIS histórico
-   Antiguos titulares, empresas y correos
-   Reverse WHOIS
-   Certificate Transparency
-   Subdominios
-   Infraestructura relacionada
-   Tecnologías web
-   Histórico del sitio
-   Documentos indexados
-   Correos electrónicos
-   GitHub
-   Threat Intelligence
-   Relaciones entre dominios, IP, ASN, NS y MX

### 🌍 Investigación de direcciones IP

-   ASN y netblock
-   Organización / proveedor
-   RDAP / WHOIS
-   Routing y BGP
-   Reverse DNS
-   Passive DNS
-   Dominios relacionados
-   Servicios observados por terceros
-   Reputación y Threat Intelligence
-   Indicadores de VPN, Proxy y Tor
-   Hosting / Datacenter

RASTRO-WEB evita equiparar automáticamente conceptos diferentes:

``` text
ASIGNACIÓN DE RED
        ≠
PROVEEDOR DE HOSTING
        ≠
VPN / PROXY
        ≠
ATRIBUCIÓN A UNA PERSONA
```

Una coincidencia de infraestructura es un **pivote técnico**, no una
prueba automática de identidad o titularidad.

### 🔗 Análisis de URLs

Las URLs se analizan localmente antes de abrir fuentes externas. La
herramienta puede extraer protocolo, hostname, dominio registrable,
puerto, path, query string, parámetros, fragmento, valores codificados,
Punycode/IDN, URLs anidadas y hosts expresados mediante una dirección
IP.

Esto permite estudiar la estructura de un enlace sospechoso sin
necesidad de visitarlo directamente.

------------------------------------------------------------------------

## 🕰️ WHOIS histórico

RASTRO-WEB incluye un flujo específico para reconstruir la evolución
histórica de un dominio.

Permite registrar:

-   Antiguos titulares
-   Empresas
-   Correos electrónicos
-   Nameservers
-   Cambios de infraestructura
-   Fechas históricas / snapshots

``` text
example.com
   │
   ├── ANTIGUO_TITULAR → Example Person
   ├── ANTIGUA_EMPRESA → Example Ltd.
   ├── EMAIL_HISTORICO → admin@example.net
   └── NS_HISTORICO → ns1.oldprovider.net
```

La herramienta diferencia entre la **fecha histórica a la que pertenece
el dato** y la **fecha en la que el investigador lo incorpora al
expediente**.

------------------------------------------------------------------------

## 🧠 Captura inteligente

Muchas plataformas OSINT no permiten que otra página web lea
automáticamente sus resultados debido a las restricciones de seguridad
del navegador.

RASTRO-WEB utiliza un sistema de **captura inteligente**. El
investigador puede copiar resultados obtenidos en una fuente externa y
pegarlos en:

``` text
CAPTURA INTELIGENTE → GRAFO
```

La herramienta intenta detectar y estructurar localmente IPv4/IPv6, ASN,
emails, URLs, dominios, subdominios, registros A/AAAA, MX, NS, CNAME,
PTR, proveedores, indicadores VPN/Proxy/Tor y datos de WHOIS histórico.

La captura puede asociarse a una fuente concreta y marcarse como
**observada, histórica o inferida**.

------------------------------------------------------------------------

## 🧾 Procedencia y corroboración

Los hallazgos pueden conservar información sobre su procedencia.

``` text
OBSERVADO
HISTÓRICO
INFERIDO
MANUAL
CORROBORADO
```

Si el mismo dato se incorpora desde diferentes fuentes, RASTRO-WEB puede
conservar varias procedencias sin crear necesariamente nodos duplicados
y marcar el hallazgo como **CORROBORADO**.

------------------------------------------------------------------------

## 🔗 Grafo de investigación

Los hallazgos pueden representarse mediante un grafo interactivo.

``` text
example.com
      │
      │ RESUELVE_A
      ▼
203.0.113.10
      │
      │ PERTENECE_ASN
      ▼
   AS64500
      │
      │ PROVEEDOR
      ▼
 Example Hosting
```

El grafo permite seleccionar nodos, crear relaciones, arrastrar y
reorganizar elementos, aplicar zoom, desplazar el lienzo, investigar un
hallazgo como nuevo pivote, acceder a acciones rápidas según el tipo de
nodo y exportar el grafo como SVG.

> **Cada hallazgo puede convertirse en un nuevo punto de
> investigación.**

------------------------------------------------------------------------

## 📋 Control de investigación

RASTRO-WEB incluye un panel de seguimiento para visualizar qué áreas de
la investigación ya se han trabajado.

``` text
✓ Identificación
✓ DNS / Infraestructura
✓ WHOIS histórico
○ Subdominios / CT
○ Threat Intelligence
○ Histórico web
```

También puede indicar un **SIGUIENTE PASO RECOMENDADO** para facilitar
un flujo de trabajo más ordenado.

------------------------------------------------------------------------

## ⭐ Hallazgos relevantes

Los elementos especialmente importantes pueden marcarse con `★`.

Los hallazgos destacados reciben mayor protagonismo en el resumen final
de la investigación.

------------------------------------------------------------------------

## 🕐 Cronología

La herramienta construye una cronología a partir de los hallazgos
almacenados y permite diferenciar entre:

-   Fecha histórica del dato
-   Fecha de observación
-   Fecha de incorporación al expediente

Resulta especialmente útil para reconstruir cambios relacionados con
WHOIS, DNS e infraestructura.

------------------------------------------------------------------------

## 📝 Resumen para el investigador

RASTRO-WEB genera una síntesis en lenguaje natural utilizando los datos
almacenados en el expediente.

El resumen puede recoger el indicador investigado, infraestructura
identificada, ASN y proveedores, señales VPN/Proxy/Tor, dominios
relacionados, WHOIS histórico, correos históricos, hallazgos relevantes,
datos corroborados y comprobaciones pendientes.

El resumen se genera **localmente en el navegador** y no necesita enviar
el expediente a un servicio externo de IA.

------------------------------------------------------------------------

## 🛠️ Fuentes OSINT

RASTRO-WEB facilita el acceso y los pivotes hacia diferentes servicios y
plataformas OSINT, entre ellos:

**RDAP · RIPEstat · SecurityTrails · Whoxy · WhoisXML API · Censys ·
Shodan · urlscan.io · VirusTotal · AbuseIPDB · IPinfo · IPcost/IPme ·
Scamalytics · DNSDumpster · DNSlytics · Robtex · Wayback Machine ·
Certificate Transparency · Hurricane Electric BGP Toolkit · ViewDNS**

Cuando una fuente admite una consulta directa mediante URL, RASTRO-WEB
intenta abrirla con el **indicador objetivo ya incorporado**. Cuando
esto no es posible de forma fiable, utiliza un flujo asistido
facilitando la copia del indicador.

> RASTRO-WEB no está afiliado ni asociado con los servicios mencionados.
> Algunas fuentes pueden requerir registro, API key, suscripción o estar
> sujetas a sus propias limitaciones y condiciones de uso.

------------------------------------------------------------------------

## 🔐 Privacidad y funcionamiento local

RASTRO-WEB sigue una filosofía **local-first**.

``` text
INDICADOR
    ↓
RASTRO-WEB
    ↓
PROCESAMIENTO LOCAL
```

La aplicación no dispone de backend propio. El indicador únicamente se
comunica a una fuente externa cuando el investigador decide abrir dicha
fuente.

Las notas, hallazgos, relaciones y otros datos de trabajo se almacenan
localmente en el navegador mediante `localStorage`.

------------------------------------------------------------------------

## 🚀 Instalación y uso

No requiere instalación.

1.  Descarga `RASTRO-WEB.html`.
2.  Abre el archivo con un navegador moderno.
3.  Introduce un **dominio, IP o URL**.
4.  Pulsa **ANALIZAR INDICADOR**.
5.  Utiliza las fuentes y pivotes propuestos.
6.  Incorpora los resultados relevantes mediante la captura inteligente.
7.  Consulta el grafo, la cronología y el resumen de investigación.

Compatible con navegadores modernos como Chrome, Firefox, Edge o Brave.

------------------------------------------------------------------------

## 💻 Arquitectura

RASTRO-WEB está desarrollada como una aplicación cliente:

``` text
HTML
CSS
JavaScript
```

No requiere Python, Node.js, Docker, base de datos, servidor web ni
backend para su funcionamiento básico.

La intención es mantener una herramienta **sencilla, portable y fácil de
ejecutar en un entorno de investigación**.

------------------------------------------------------------------------

## ⚠️ Limitaciones

Una aplicación HTML ejecutada en el navegador no puede leer libremente
el contenido mostrado por otras páginas web debido, entre otros
mecanismos, a **Same-Origin Policy** y **CORS**.

Por ello, determinadas fuentes siguen un flujo asistido:

``` text
FUENTE EXTERNA
      ↓
COPIAR RESULTADOS
      ↓
CAPTURA INTELIGENTE
      ↓
RASTRO-WEB
      ↓
HALLAZGOS / GRAFO
```

Los resultados de terceros pueden cambiar, contener errores, quedar
obsoletos o interpretar de forma diferente determinados indicadores. Los
hallazgos relevantes deben ser contrastados y contextualizados.

------------------------------------------------------------------------

## ⚖️ Uso responsable

RASTRO-WEB está orientada a investigación OSINT, análisis de fuentes
abiertas, ciberseguridad, investigación digital, análisis de fraude,
formación e investigación académica y apoyo a investigaciones legítimas.

El usuario es responsable de utilizar la herramienta conforme a la
legislación aplicable, las autorizaciones correspondientes y las
condiciones de uso de las fuentes consultadas.

La información obtenida mediante OSINT debe ser **verificada,
contextualizada y documentada** antes de utilizarse para realizar
atribuciones o tomar decisiones.

------------------------------------------------------------------------

## 📌 Filosofía del proyecto

``` text
RECOGER
   ↓
CONTRASTAR
   ↓
RELACIONAR
   ↓
DOCUMENTAR
```

El objetivo no es sustituir al investigador.

El objetivo es ayudarle a **seguir, organizar y documentar el rastro de
una investigación**.

------------------------------------------------------------------------

## 🧩 Proyecto RASTRO

RASTRO-WEB forma parte de **RASTRO**, un proyecto de herramientas
especializadas para investigación OSINT ejecutables directamente desde
el navegador.

------------------------------------------------------------------------

## 👤 Autor

**S3GAD3**

Proyecto desarrollado con fines de investigación OSINT, análisis técnico
y formación.

------------------------------------------------------------------------

**RASTRO-WEB**

*Follow the data. Keep the trace.*
