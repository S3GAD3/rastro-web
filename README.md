# RASTRO · WEB — Kit de Análisis de Infraestructura OSINT

**Licencia:** MIT · ver [`LICENSE`](./LICENSE)

**RASTRO-WEB** es una aplicación web de una sola página (single-file HTML/CSS/JS) pensada como **puesto de trabajo OSINT** para investigadores de cibercrimen y ciberinteligencia. Dado un **dominio**, una **dirección IP** o una **URL**, la herramienta detecta automáticamente el tipo de indicador y genera de forma instantánea decenas de consultas y accesos directos a fuentes públicas (WHOIS/RDAP, DNS, Certificate Transparency, Wayback Machine, Shodan/Censys, threat intelligence, redes sociales, GitHub, dorks de Google/Bing, etc.), a la vez que ofrece un **espacio de caso** (notas, hallazgos, grafo de relaciones, cronología y resumen del expediente) para documentar la investigación.

> ⚠️ **RASTRO-WEB no es un escáner.** No realiza peticiones activas contra el objetivo, no evita CORS, no usa proxies ni claves de API propias. Es un **generador de accesos y consultas** hacia fuentes OSINT públicas: quien decide abrir cada fuente es el investigador, y solo entonces el indicador "sale" del navegador hacia ese tercero.

> **Versión documentada: v2.2.** Incluye importación/exportación de expediente (con exportación cifrada AES-GCM y hash de integridad SHA-256), tema claro/oscuro y ajustes de visualización para tablet, además de las funciones OSINT originales.

---

## 1. ¿Para quién es esta herramienta?

Está pensada para investigadores policiales, analistas de ciberinteligencia, equipos CERT/SOC o profesionales de *threat intelligence* que necesitan:

- Acelerar la fase de **triage** de un indicador (dominio, IP o URL) sin memorizar decenas de URLs de servicios OSINT.
- Mantener un **expediente ordenado** (hallazgos, relaciones, notas, cronología) por cada indicador investigado.
- Trabajar con **trazabilidad y evidencias**, distinguiendo dato observado, dato histórico y dato inferido.
- Operar en un entorno **100% local**, sin depender de servidores propios ni de conexiones API de terceros.

---

## 2. Valoración como herramienta de inteligencia

### Utilidad
- **Ahorro de tiempo real en la fase de reconocimiento pasivo**: sustituye una checklist mental de "qué mirar" (WHOIS, DNS, CT logs, Wayback, Shodan, reputación, redes sociales...) por un panel ya organizado y con consultas precargadas.
- **Estandariza el proceso**: dos investigadores distintos analizando el mismo indicador seguirán el mismo flujo de categorías, reduciendo el riesgo de "olvidar" una fuente relevante.
- **Gestión de caso incorporada**: hallazgos, grafo de relaciones, cronología y resumen automático conviven con el motor de búsqueda, algo que normalmente exige varias herramientas distintas (bloc de notas + Maltego + hoja de cálculo).
- **Generador de dorks integrado**: construye combinaciones `site:` / `filetype:` / `inurl:` sin que el investigador tenga que recordar la sintaxis.

### Facilidad de uso
- Curva de aprendizaje muy baja: un único campo de entrada, detección automática de tipo, y navegación por pestañas/categorías con niveles de profundidad (**Esencial / Ampliado / Completo**) que evitan saturar al usuario novato.
- El **panel de control de investigación** (barra de progreso) orienta sobre qué pasos quedan pendientes, lo que resulta muy útil para investigadores en formación.
- El **resumen para el investigador** en lenguaje natural facilita el volcado inicial a un informe o atestado, aunque **debe revisarse siempre**, ya que es una plantilla determinista y no un análisis semántico real.

### Limitaciones a tener en cuenta
- **El almacenamiento en vivo sigue siendo local (`localStorage`)**: no hay sincronización entre equipos ni copia de seguridad automática; si se borra el caché del navegador se pierde el expediente. Desde la v2.2 la app avisa periódicamente si llevas más de 15 minutos sin exportar, pero conviene **exportar con frecuencia** (Markdown/JSON/TXT o JSON cifrado).
- **No verifica ni contrasta datos por sí misma**: son enlaces hacia terceros; la fiabilidad depende de la fuente externa consultada en cada momento.
- **El cifrado cubre la exportación, no el `localStorage` en vivo**: mientras trabajas, notas y hallazgos siguen sin cifrar en el navegador; usa la exportación cifrada para archivar o compartir el expediente fuera del equipo, y no compartas el perfil del navegador sin cifrado de disco.
- **Generación de typosquatting es heurística**: las variantes de dominio son hipótesis de trabajo, no evidencia de registro real.
- Varias fuentes (Wappalyzer, WhoisXML, Censys Platform, IPcost) no ofrecen *deep-link* público estable, por lo que RASTRO abre la fuente y copia el valor al portapapeles para pegarlo manualmente ("modo asistido").

### Veredicto
Como herramienta de **primera línea para reconocimiento pasivo y organización de expediente**, RASTRO-WEB cumple muy bien su función: es rápida, no requiere instalación ni backend, y reduce fricción operativa. No sustituye herramientas de correlación avanzada (Maltego, SpiderFoot, i2 Analyst's Notebook) ni el juicio del investigador para verificar y corroborar cada hallazgo con al menos dos fuentes independientes, algo que la propia herramienta recuerda en sus avisos.

---

## 3. Características principales

| Módulo | Descripción |
|---|---|
| **Detección automática de indicador** | Identifica si el texto introducido es un `DOMAIN`, una `IP` (v4/v6, con clasificación de ámbito: pública, privada, CGNAT, documentación, reservada) o una `URL`, y normaliza el valor. |
| **Catálogo de fuentes por categoría** | WHOIS/RDAP, DNS, Certificate Transparency, histórico (Wayback), tecnologías, infraestructura/ASN, dominios relacionados, typosquatting, threat intelligence, GitHub, redes sociales, prensa, identidad corporativa (para dominios); identificación, proveedor/VPN/proxy/Tor, RDAP, ASN/BGP, exposición en Internet, reverse DNS, reputación, histórico (para IPs); análisis local, reputación, observación web, histórico, host, parámetros/redirecciones (para URLs). |
| **Niveles de profundidad** | *Esencial* (4 resultados por bloque), *Ampliado* (9) y *Completo* (todos), para no saturar al investigador en una primera pasada. |
| **Constructor de dorks** | Genera consultas `site:` / `filetype:` / `inurl:` / exclusión de texto sin modificar el dominio objetivo. |
| **Captura inteligente → grafo** | Permite pegar resultados de otras herramientas (DNS, WHOIS/RDAP, SecurityTrails, urlscan, Censys, RIPEstat, Whoxy...) y detecta localmente IPs, ASN, correos, URLs y hostnames para incorporarlos como hallazgos. |
| **Hallazgos y evidencias** | Alta manual de hallazgos con tipo, relación, fuente y estado de evidencia (`MANUAL`, `OBSERVADO`, `HISTÓRICO`, `INFERIDO`). |
| **Grafo de investigación** | Visualización SVG interactiva (zoom, arrastre, relaciones) entre el indicador raíz y los hallazgos del expediente. |
| **Cronología del expediente** | Ordena hallazgos según fecha histórica u fecha de incorporación. |
| **Resumen para el investigador** | Síntesis en lenguaje natural generada localmente (sin IA externa) a partir de los hallazgos y relaciones. |
| **Exportación de expediente** | Descarga en Markdown, JSON, TXT o **JSON cifrado (`.enc`, AES-GCM 256 + PBKDF2)**. Cada exportación incluye un **hash SHA-256** de integridad. |
| **Importación de expediente** | Recupera un caso desde un fichero exportado (JSON o `.enc`); detecta el formato automáticamente y pide la contraseña si está cifrado. |
| **Recordatorio de backup** | Aviso no intrusivo en la barra de estado si pasan más de 15 minutos desde la última exportación con hallazgos pendientes de respaldar. |
| **Tema claro/oscuro** | Alternable manualmente y con detección de preferencia del sistema; se recuerda entre sesiones. |
| **Historial y favoritos** | Registro local de indicadores analizados y consultas marcadas como favoritas. |
| **Pivotes** | A partir de un hallazgo (IP, ASN, correo, NS...) genera nuevas vías de investigación relacionadas. |

---

## 4. Requisitos

- Un **navegador web moderno** (Chrome, Firefox, Edge o Safari actualizados). No requiere instalación, servidor, backend, base de datos ni claves de API.
- Conexión a Internet **solo** para abrir las fuentes externas que decidas consultar; el análisis local (detección de tipo, parseo de capturas, grafo, resumen) funciona sin conexión.
- JavaScript habilitado.

---

## 5. Instalación y puesta en marcha

1. Descarga el fichero `index.html` de este repositorio (o clona el repositorio completo).
2. Ábrelo directamente con doble clic o desde el navegador (`archivo:///ruta/index.html`), o publícalo en cualquier servidor web estático / GitHub Pages si prefieres acceder por URL.
3. No hay build, dependencias ni instalación de paquetes: es un único fichero autocontenido.

```bash
git clone https://github.com/<usuario>/<repositorio>.git
cd <repositorio>
# Abre index.html con tu navegador, o sirve la carpeta con cualquier servidor estático:
python3 -m http.server 8080
# y visita http://localhost:8080/index.html
```

---

## 6. Guía de uso paso a paso

### Paso 1 · Introducir el indicador
En el campo **"Indicador web objetivo"**, introduce un dominio (`example.com`), una IP (`185.199.108.153`) o una URL completa (`https://sub.example.com/ruta?id=123`) y pulsa **▶ Analizar indicador**. RASTRO-WEB detecta automáticamente el tipo y muestra un panel con el análisis estructural local (host, dominio registrable, versión de IP, ámbito de red, señales locales como puerto no estándar, punycode o credenciales embebidas en la URL, etc.).

### Paso 2 · Elegir el nivel de profundidad
Empieza siempre por **Esencial**. Si necesitas más fuentes por categoría, cambia a **Ampliado** o **Completo** desde el selector de nivel. Esto no vuelve a consultar nada externo: solo cambia cuántos resultados se muestran por bloque.

### Paso 3 · Recorrer las categorías de fuentes
Cada categoría (WHOIS, DNS, histórico, infraestructura, threat intelligence, redes sociales...) se despliega como un acordeón. Cada entrada indica su modo:
- **DIRECTA**: abre la consulta ya precargada con el valor del indicador.
- **ASISTIDA**: abre la fuente externa y copia el valor al portapapeles porque el servicio no admite enlaces directos parametrizados.
- **BÚSQUEDA**: lanza un dork en el buscador seleccionado (Google, Bing, DuckDuckGo, GitHub...).

Puedes seleccionar varias fuentes con la casilla de cada tarjeta y abrirlas todas a la vez con **"Abrir seleccionadas"**, o filtrar el listado con el buscador de fuentes (ej. escribir `VPN`, `histórico`, `DNS`).

### Paso 4 · Documentar hallazgos
En la sección **HALLAZGOS**, añade manualmente cada dato relevante que descubras en las fuentes externas (IP, subdominio, correo, ASN, titular, proveedor, etc.), indicando su relación con el indicador, la fuente concreta y el estado de la evidencia (observado, histórico, inferido o manual).

### Paso 5 · Captura inteligente (opcional)
Si copias resultados en bruto de otra herramienta (registros DNS, salida de RIPEstat, WHOIS, etc.) en el cuadro de **"Captura inteligente"**, RASTRO-WEB detecta localmente IPs, ASN, correos, URLs y hostnames, y puede añadirlos automáticamente al grafo si mantienes activada la opción "AUTO → GRAFO al pegar".

### Paso 6 · Visualizar el grafo de relaciones
En **GRAFO DE INVESTIGACIÓN** puedes ver el indicador raíz conectado con todos los hallazgos incorporados, crear relaciones adicionales entre nodos, investigar un nodo concreto (abre nuevas fuentes centradas en ese valor) y exportar el grafo como SVG.

### Paso 7 · Generar el resumen y exportar el expediente
Pulsa **"Actualizar resumen"** para generar una síntesis narrativa local del expediente (titulares, proveedores, señales de privacidad, nameservers históricos, hallazgos relevantes, pendientes según el flujo de trabajo…). Cuando el expediente esté completo, usa **"Exportar informe"** para descargarlo en Markdown, JSON, TXT o **JSON cifrado (`.enc`)**. Todas las exportaciones incluyen un hash SHA-256 de integridad; la opción cifrada pide una contraseña (con confirmación) y usa AES-GCM 256 con derivación PBKDF2 — sin ella no podrás recuperar el fichero, así que guárdala en un gestor de contraseñas.

### Paso 8 · Importar un expediente
Usa **"Importar expediente"** para recuperar un caso previamente exportado en otro equipo o sesión. Selecciona el fichero `.json` o `.enc`; si está cifrado, la aplicación pedirá la contraseña. Los hallazgos, notas, grafo y favoritos del fichero se cargan y quedan disponibles como si acabaras de analizar ese indicador.

### Paso 9 · Pivotar sobre nuevos hallazgos
Repite el proceso con cualquier hallazgo relevante (una IP nueva, un correo, un ASN) usando el módulo **"INVESTIGAR HALLAZGO"** para abrir nuevas vías de investigación sin perder el contexto del caso original.

### Paso 10 · Tema claro/oscuro
El botón **"🌙 Modo oscuro"** (junto a "Exportar informe") alterna la paleta visual manteniendo el mismo estilo y logotipo. Por defecto sigue la preferencia del sistema operativo/navegador la primera vez, y a partir de ahí recuerda tu elección.

---

## 7. Buenas prácticas operativas

- **Corrobora siempre con al menos dos fuentes independientes** antes de dar por buena una atribución (proveedor, titular, VPN/proxy, relación de infraestructura). RASTRO-WEB señala esto explícitamente en sus avisos, pero la decisión final es del investigador.
- **Distingue dato observado de dato histórico**: un WHOIS histórico o un nameserver antiguo no implica titularidad actual.
- **Exporta el expediente con frecuencia** (Markdown/JSON) para no depender únicamente del `localStorage` del navegador; usa la variante **cifrada (`.enc`)** si el fichero va a salir del equipo (archivo, envío a un compañero, adjunto a un atestado).
- **Verifica el hash SHA-256** incluido en cada exportación antes de dar por buena la integridad de un expediente recibido de otra persona.
- **Guarda la contraseña de cifrado en un gestor de contraseñas**: RASTRO-WEB no la almacena ni puede recuperarla; si se pierde, el `.enc` es irrecuperable.
- **No compartas el navegador/perfil** en el que trabajas con RASTRO-WEB sin cifrado de disco, ya que notas y hallazgos quedan almacenados localmente sin cifrar mientras trabajas (el cifrado se aplica al exportar, no al `localStorage` en vivo).
- **Respeta la legislación aplicable** a tu investigación y los términos de uso de cada fuente externa que consultes; RASTRO-WEB no autoriza ni legitima el uso de ninguna fuente, solo facilita el acceso a servicios OSINT públicos.

---

## 8. Privacidad y arquitectura técnica

- **100% cliente**: no existe backend ni servidor propio. Todo el procesamiento (detección de tipo, parseo de capturas, construcción del grafo, generación del resumen, cifrado/descifrado) ocurre en el navegador mediante la **Web Crypto API** nativa.
- **Sin telemetría ni llamadas externas automáticas**: el indicador solo sale del dispositivo cuando el investigador decide abrir manualmente una fuente externa.
- **Persistencia en `localStorage`**: historial, favoritos, notas, hallazgos y posiciones del grafo se guardan localmente por expediente, sin sincronización en la nube.
- **Cifrado de exportación**: AES-GCM de 256 bits con clave derivada por PBKDF2 (150.000 iteraciones) a partir de la contraseña introducida; el fichero `.enc` incluye la sal y el vector de inicialización, ambos necesarios (junto con la contraseña) para descifrar.
- **Integridad**: cada exportación incorpora un hash SHA-256 del contenido, calculado localmente, para poder verificar que el expediente no se ha alterado tras generarse.
- **Sin claves de API ni credenciales**: todas las fuentes se consultan como lo haría cualquier usuario desde su navegador; no hay scraping automatizado ni bypass de CORS.

> ⚠️ La Web Crypto API (`crypto.subtle`) requiere un **contexto seguro** (HTTPS, `localhost` o, según el navegador, `file://`). Si el botón de exportación/importación cifrada indica que el cifrado no está disponible, sirve la aplicación por HTTP local (ver sección 5) en lugar de abrir el HTML directamente con doble clic.

---

## 9. Limitaciones conocidas

- No realiza resolución DNS, escaneo de puertos ni ninguna consulta activa contra el objetivo.
- No verifica la disponibilidad ni vigencia de los enlaces a terceros; algunos servicios pueden cambiar su interfaz o requerir cuenta/API key propia del investigador.
- La generación de variantes de typosquatting es heurística local y no comprueba si esos dominios existen realmente.
- El resumen narrativo es una plantilla basada en reglas, no un modelo de lenguaje: no interpreta matices ni realiza inferencias más allá de lo programado.
- El cifrado protege los ficheros **exportados**, no los datos mientras se trabaja en `localStorage`; una migración a un almacenamiento cifrado en vivo (o a IndexedDB) queda pendiente como mejora futura, ya que exigiría un refactor mayor del motor de guardado.
- No incluye manifiesto PWA ni funcionamiento instalable/offline explícito, para mantener el proyecto como un único fichero HTML autocontenido y fácil de distribuir.

---

## 10. Aviso legal

Herramienta de uso interno autorizado para investigación de ciberinteligencia y cibercrimen. El usuario es responsable del uso que haga de las fuentes externas enlazadas y del cumplimiento de la legislación aplicable a su investigación (protección de datos, autorización judicial cuando proceda, términos de servicio de terceros, etc.). RASTRO-WEB no almacena ni transmite datos a ningún servidor de terceros por sí misma.

---

## 11. Contribuir

Al tratarse de un fichero HTML autocontenido, cualquier contribución (nuevas fuentes OSINT, corrección de enlaces caducados, mejoras de UI) puede proponerse mediante *pull request* directamente sobre `index.html`. Se recomienda documentar en la descripción del PR qué categoría o fuente se añade/modifica y por qué.

## 12. Licencia

Este proyecto se distribuye bajo licencia **MIT** — ver el fichero [`LICENSE`](./LICENSE). En resumen: puedes usar, copiar, modificar, fusionar, publicar y distribuir el software libremente, incluso con fines comerciales, siempre que conserves el aviso de copyright y la licencia. Se entrega **"tal cual"**, sin garantía de ningún tipo; el aviso legal de la sección 10 sobre el uso responsable de las fuentes OSINT enlazadas sigue aplicando independientemente de la licencia de software.
