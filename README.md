# RASTRO WEB

**Herramienta OSINT para investigar dominios, direcciones IP y URL desde el navegador.**
Un único fichero HTML, sin servidor, sin instalación y sin claves de API.

![Licencia MIT](https://img.shields.io/badge/licencia-MIT-blue)
![Versión 3.0](https://img.shields.io/badge/versi%C3%B3n-3.0-1259b8)
![Sin backend](https://img.shields.io/badge/backend-ninguno-0a2452)

RASTRO WEB reúne en una sola pantalla las tres cosas que un investigador hace al recibir un indicador web: **consultar** los datos técnicos públicos, **pivotar** hacia fuentes externas y **documentar** lo hallado con su procedencia. Todo el trabajo se guarda en el propio navegador.

---

## Índice

- [Para qué sirve](#para-qué-sirve)
- [Valor para el investigador de ciberinteligencia](#valor-para-el-investigador-de-ciberinteligencia)
- [Puesta en marcha](#puesta-en-marcha)
- [Cómo funciona](#cómo-funciona)
  - [1. Reconocimiento directo](#1-reconocimiento-directo)
  - [2. Fuentes externas](#2-fuentes-externas)
  - [3. Evidencias](#3-evidencias)
  - [4. Grafo de relaciones](#4-grafo-de-relaciones)
  - [5. Informe y exportación](#5-informe-y-exportación)
- [Privacidad y OPSEC](#privacidad-y-opsec)
- [Limitaciones](#limitaciones)
- [Detalles técnicos](#detalles-técnicos)
- [Uso responsable](#uso-responsable)
- [Licencia y autor](#licencia-y-autor)

---

## Para qué sirve

Se introduce un indicador y la herramienta detecta sola de qué tipo es:

| Tipo | Ejemplo | Qué se analiza |
|---|---|---|
| Dominio | `ejemplo.com`, `sub.ejemplo.co.uk` | Registro, DNS, subdominios, alojamiento, histórico, dominios similares |
| IP | `185.199.108.153`, `2001:4860:4860::8888` | Asignación del rango, operador, geolocalización, anonimización, servicios expuestos |
| URL | `https://sub.ejemplo.com/login?next=…` | Anatomía de la URL sin visitarla, más todo lo anterior sobre su host |

A partir de ahí ofrece cinco áreas de trabajo: **Reconocimiento**, **Fuentes**, **Evidencias**, **Grafo** e **Informe**.

El enfoque es de **reconocimiento pasivo**: la herramienta nunca envía peticiones al objetivo ni escanea puertos. Solo pregunta a servicios públicos de terceros lo que ya saben de él.

---

## Valor para el investigador de ciberinteligencia

**Reduce el tiempo hasta el primer dato útil.**
Lo habitual es abrir una decena de webs y copiar el indicador en cada una. Aquí un solo botón lanza las consultas básicas y devuelve una ficha operativa con registrador, fecha de creación, IP actual, operador de red, país, proveedor de DNS y de correo.

**Orienta los requerimientos.**
El resumen identifica a quién dirigirse para obtener datos de titularidad o de conexión: el registrador y su contacto de abuso, el proveedor de alojamiento, el titular del rango IP. Si el dominio está detrás de una CDN como Cloudflare, lo advierte: la IP visible no es la del servidor real y esa información solo la tiene el proveedor.

**Avisa de lo que cambia la interpretación.**
Señala los dominios registrados hace menos de 90 días, los suspendidos, las IP que son nodo de salida de Tor, las redes habituales de servicios VPN, la falta de DMARC (dominio suplantable por correo) y los rasgos sospechosos de una URL: credenciales embebidas, punycode, puertos no estándar o redirecciones anidadas en parámetros.

**Conserva la procedencia de cada dato.**
Cada hallazgo guarda de qué fuente salió, cuándo se incorporó y con qué carácter: observado, histórico, inferido o introducido a mano. Si dos fuentes independientes aportan el mismo dato, pasa a estar corroborado. Esto permite separar en el informe el hecho de la hipótesis.

**Sella las consultas en el tiempo.**
Cada resultado directo lleva fecha y hora UTC y el nombre del servicio consultado. Los informes exportados incluyen un hash SHA-256 que permite comprobar después que no se han alterado.

**Hace visible la infraestructura.**
El grafo muestra cómo se encadenan dominio, IP, sistema autónomo, servidores de nombres, correos e identidades. Sirve para detectar infraestructura compartida entre varios indicadores y para explicar el caso a terceros.

**No depende de nadie.**
No hay cuentas, servidor ni telemetría. El fichero puede llevarse en un pendrive, usarse en un equipo aislado de la red corporativa o auditarse línea a línea antes de emplearlo.

**Mantiene la cautela jurídica.**
Los textos de la herramienta recuerdan de forma constante que compartir IP, sistema autónomo o servidor de nombres es una pista técnica y no una prueba de titularidad, y que un WHOIS histórico acredita un dato en una fecha, no la situación actual.

---

## Puesta en marcha

No requiere instalación.

1. Descarga `index_rastro_web.html`.
2. Ábrelo en un navegador actual (Chrome, Edge, Firefox o Safari).
3. Escribe un dominio, una IP o una URL y pulsa **Analizar**.

También puede publicarse en GitHub Pages o en cualquier servidor de ficheros estáticos.

**Enlace directo a un indicador.** Añadiendo `#q=` a la dirección se abre ya analizado:

```
index_rastro_web.html#q=ejemplo.com
```

**Atajos de teclado**

| Tecla | Acción |
|---|---|
| `/` | Ir al campo de búsqueda |
| `Ctrl` + `K` | Filtrar fuentes |
| `Esc` | Cerrar un panel o salir del grafo a pantalla completa |

---

## Cómo funciona

### 1. Reconocimiento directo

Consultas en vivo a servicios públicos que no piden clave. Se ejecutan una a una o todas con **Ejecutar todo**; nunca se lanzan solas. Los hallazgos de las consultas básicas (DNS, registro, hosting) pueden añadirse automáticamente al expediente; los de las consultas que devuelven listas largas (subdominios, urlscan, dominios similares) se añaden a mano.

**Para un dominio**

| Consulta | Qué devuelve | Servicio |
|---|---|---|
| DNS en vivo | Registros A, AAAA, CNAME, NS, MX, TXT, SOA y CAA; SPF y DMARC; proveedor de DNS y de correo | DNS sobre HTTPS de Google, con Cloudflare como respaldo |
| Registro del dominio | Registrador, fechas de creación y expiración, estado, servidores de nombres, contacto de abuso y registrante si está publicado | RDAP a través de rdap.org |
| Hosting | Para cada IP (máximo tres): prefijo, sistema autónomo, operador, ubicación aproximada, DNS inverso, puertos abiertos y CVE asociados | RIPEstat, ipwho.is, Shodan InternetDB |
| Subdominios | Nombres incluidos en certificados TLS emitidos para el dominio | crt.sh |
| Histórico web | Primera y última captura archivada | Internet Archive |
| Observaciones | Análisis previos con IP, red, servidor y títulos de página | urlscan.io |
| Dominios similares | Variantes tipográficas y de extensión que existen en DNS | DNS sobre HTTPS |

**Para una IP**

| Consulta | Qué devuelve | Servicio |
|---|---|---|
| Red | Prefijo anunciado, sistema autónomo y operador | RIPEstat |
| Asignación del rango | Titular del bloque, país y contacto de abuso | RDAP a través de rdap.org |
| Geolocalización e ISP | País, ciudad aproximada, ISP y organización | ipwho.is |
| DNS inverso y Tor | Registro PTR y comprobación en la lista de nodos de salida de Tor Project | DNS sobre HTTPS |
| Puertos y servicios | Puertos, nombres de host, software y CVE ya indexados | Shodan InternetDB |

**Para una URL** se muestra primero su anatomía (protocolo, host, puerto, ruta, parámetros y avisos estructurales), calculada en local y sin visitarla. Después se aplican las consultas de dominio o de IP sobre su host.

Con los resultados se compone una **ficha operativa** y un indicador de **cobertura** que muestra qué fases de la investigación están hechas y cuáles faltan.

Si un servicio no responde o bloquea la petición, la consulta lo indica y ofrece el enlace para abrir esa fuente a mano.

### 2. Fuentes externas

Catálogo de enlaces a herramientas de terceros con el indicador ya cargado, agrupados por fase de investigación.

- **Dominio:** registro y titularidad, DNS actual e histórico, subdominios, infraestructura y tecnología, reputación, histórico web, correos e identidades, contenido indexado, menciones y dominios similares.
- **IP:** asignación y operador, anonimización (VPN, proxy, Tor), dominios alojados, servicios expuestos, reputación y menciones.
- **URL:** reputación, observación sin visitar, histórico y menciones, más el catálogo completo de su host.

Cada fuente es de uno de estos tres tipos:

| Tipo | Comportamiento |
|---|---|
| Directa | Abre la consulta ya hecha |
| Asistida | Copia el dato al portapapeles y abre la fuente para pegarlo |
| Búsqueda | Lanza un dork en un buscador |

Funciones del catálogo:

- Dos niveles de detalle: **Esencial** (las fuentes de primera consulta) y **Completo**.
- Filtro por texto, selección múltiple para abrir varias a la vez y marcado de favoritas.
- Las fuentes ya abiertas quedan atenuadas y se registran en el informe con su hora.
- Constructor de dorks con texto exacto, tipo de fichero, `inurl` y exclusiones.

Entre las fuentes incluidas están ICANN Lookup, Whoxy, SecurityTrails, ViewDNS, crt.sh, urlscan, Shodan, Censys, VirusTotal, AbuseIPDB, Spur, ExoneraTor, BuiltWith, Hunter y Wayback Machine. Para dominios `.es` se añade el registro oficial de Red.es, y entre los dorks hay uno orientado a localizar el aviso legal y la identificación fiscal del responsable del sitio.

### 3. Evidencias

**Hallazgos.** Cada dato del expediente tiene tipo (dominio, IP, subdominio, correo, NS, MX, ASN, URL, empresa, proveedor, titular, etc.), relación con el dato del que depende, fuentes y estado:

| Estado | Significado |
|---|---|
| `OBSERVADO` | Obtenido de una fuente en el momento de la consulta |
| `HISTORICO` | Dato de una fecha pasada, por ejemplo un WHOIS archivado |
| `INFERIDO` | Deducción del investigador o hipótesis técnica |
| `MANUAL` | Introducido a mano sin fuente declarada |
| `CORROBORADO` | Aportado por dos o más fuentes distintas |

Los hallazgos se pueden marcar como relevantes para que encabecen el informe.

**Captura inteligente.** Se pega el texto copiado de cualquier fuente y la herramienta extrae en local las direcciones IP, sistemas autónomos, correos, URL, subdominios, registros DNS y datos de titularidad de un WHOIS. Se elige la fuente, el carácter del dato y, si procede, su fecha histórica.

**Pivotes.** Al pulsar cualquier hallazgo se abre un panel con las vías de investigación para ese tipo de dato: búsqueda inversa de WHOIS por correo o por nombre, dominios que comparten servidor de nombres o de correo, otros hosts del mismo sistema autónomo, registros mercantiles. Si el dato es un dominio, una IP o una URL, puede abrirse como nuevo objetivo con su propio expediente.

**Libreta y cronología.** Notas libres por expediente y una línea temporal que ordena los hallazgos por la fecha del dato o, si no la tiene, por la de incorporación.

### 4. Grafo de relaciones

Representación del expediente con el objetivo como nodo raíz.

- Los hallazgos se enlazan con el dato del que proceden: dominio → IP → sistema autónomo.
- Se pueden añadir relaciones manuales entre dos nodos cualesquiera y cambiar el tipo de relación.
- Admite arrastrar nodos, desplazar, hacer zoom y verlo a pantalla completa.
- La disposición es automática hasta que se mueve un nodo a mano; desde entonces se respetan las posiciones.
- Exporta a SVG.

### 5. Informe y exportación

El **resumen** se redacta en local a partir de los hallazgos y de las consultas directas. Recoge los datos de registro e infraestructura, las alertas, las identidades documentadas, los posibles destinatarios de requerimiento, los hallazgos relevantes, los corroborados y las fases pendientes.

Formatos de exportación:

| Formato | Contenido |
|---|---|
| Markdown / texto | Informe completo: resumen, notas, consultas directas con sello horario, hallazgos, relaciones y fuentes consultadas |
| JSON | Expediente completo, reimportable en otro equipo |
| `.enc` | El mismo JSON cifrado con contraseña |

Todas las exportaciones incorporan un hash SHA-256 de integridad. La copia cifrada usa AES-256-GCM con clave derivada por PBKDF2 (SHA-256, 150 000 iteraciones); sin la contraseña no es recuperable.

---

## Privacidad y OPSEC

**Qué se queda en el equipo.** Notas, hallazgos, historial, favoritos, posiciones del grafo y resultados de las consultas se guardan en el `localStorage` del navegador. No hay servidor propio, cuentas ni telemetría.

**Qué sale del equipo.** El indicador solo se transmite en dos casos, ambos por acción del usuario:

1. Al ejecutar una consulta directa.
2. Al abrir una fuente externa.

Las consultas directas contactan con estos servicios:

| Servicio | Uso |
|---|---|
| `dns.google`, `cloudflare-dns.com` | Resolución DNS |
| `rdap.org` y el servidor RDAP del registro o RIR correspondiente | Datos de registro |
| `stat.ripe.net` | Prefijo y sistema autónomo |
| `ipwho.is` | Geolocalización e ISP |
| `internetdb.shodan.io` | Puertos y servicios indexados |
| `crt.sh` | Certificados |
| `archive.org`, `web.archive.org` | Histórico web |
| `urlscan.io` | Análisis previos |

> **Importante:** esas peticiones salen con la dirección IP del investigador. Los terceros consultados pueden registrar qué indicador se ha preguntado y desde dónde. Si el caso lo requiere, utiliza una salida de red dedicada a la investigación.

La página se sirve con política `no-referrer`, de modo que las fuentes externas no reciben la dirección desde la que se abrieron.

---

## Limitaciones

- **Depende de terceros.** Las consultas directas funcionan mientras esos servicios permitan peticiones desde el navegador (CORS) y mantengan su acceso gratuito. Cualquiera puede cambiar sus condiciones, limitar el número de peticiones o dejar de responder. Las redes corporativas y los bloqueadores de contenido también pueden impedirlas.
- **Los datos pueden estar desactualizados.** Lo indexado por Shodan, urlscan o Wayback refleja el momento en que ellos lo observaron.
- **No todos los dominios tienen RDAP.** Algunas extensiones nacionales no lo ofrecen; en ese caso hay que usar las fuentes WHOIS del catálogo.
- **La geolocalización IP es aproximada** y sitúa al proveedor, no al usuario.
- **Las etiquetas de VPN o proxy son valoraciones de terceros** y cambian con el tiempo.
- **El dominio registrable se calcula con una lista reducida** de sufijos de varios niveles (`co.uk`, `com.es`, `com.mx`…), no con la Public Suffix List completa. En extensiones poco comunes puede identificar mal el dominio base.
- **Los enlaces del catálogo pueden quedar obsoletos** cuando las webs de destino cambian su estructura.
- **Los datos viven solo en el navegador.** Borrar los datos de navegación elimina los expedientes. Conviene exportar con regularidad; la herramienta lo recuerda.
- **Abrir varias fuentes a la vez** puede requerir permitir ventanas emergentes.

---

## Detalles técnicos

- Un único fichero HTML con CSS y JavaScript integrados, sin dependencias ni librerías externas.
- El logotipo va embebido; no se carga ningún recurso remoto al abrir la página.
- Todo el contenido recibido de terceros se inserta como texto, nunca como HTML.
- Tema oscuro y claro. Diseño adaptable a móvil y tableta.

El código está organizado en doce secciones comentadas:

```
1  Utilidades                 7  Captura inteligente
2  Estado                     8  Pivotes
3  Indicador (detección)      9  Grafo
4  Catálogo de fuentes       10  Informe y exportación
5  Reconocimiento directo    11  Interfaz
6  Hallazgos                 12  Arranque
```

**Añadir una fuente al catálogo.** En la sección 4 cada fuente es una línea con uno de estos constructores:

```js
L('Nombre', `https://servicio.tld/buscar/${e(b)}`, 'nota', true)   // directa
A('Nombre', 'https://servicio.tld/', b, 'nota')                    // asistida: copia el dato
S('Nombre', `site:${b} "texto"`, 'nota')                           // búsqueda (dork)
```

El último parámetro (`true`) hace que la fuente aparezca en el nivel Esencial.

**Añadir una consulta directa.** En la sección 5, cada módulo de `MODULES` declara cuándo se aplica, a qué servicio llama y una función `run` que devuelve los pares clave-valor, los hallazgos, los avisos y los datos para la ficha operativa.

**Almacenamiento.** Los datos se guardan bajo claves con prefijo `rd_` (`rd_findings`, `rd_notes`, `rd_recon`, `rd_history`, etc.), indexados por indicador.

---

## Uso responsable

RASTRO WEB está pensada para investigación autorizada de cibercrimen, ciberinteligencia y análisis de amenazas.

- Trabaja solo con fuentes abiertas y no realiza acciones intrusivas, pero quien la usa es responsable de cumplir la normativa aplicable y las condiciones de uso de cada servicio consultado.
- Los resultados son indicios técnicos. No acreditan por sí solos la identidad ni la responsabilidad de una persona y deben contrastarse antes de sustentar cualquier conclusión.
- Algunas fuentes externas del catálogo sí realizan acciones activas desde sus servidores, como un escaneo nuevo en urlscan. Están señaladas en la propia herramienta.

---

## Licencia y autor

Publicado bajo licencia **MIT**. Consulta el fichero [LICENSE](LICENSE).

Creado por **S3GAD3**.
