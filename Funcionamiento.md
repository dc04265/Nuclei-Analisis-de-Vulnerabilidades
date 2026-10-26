# Funcionamiento de Nuclei

> Guía de referencia basada en la salida real de `nuclei.exe -h` de la versión **3.11.1**, instalada en Windows 10. Las opciones de otras versiones pueden variar.

## 1. Cómo funciona Nuclei

Nuclei es un escáner basado en **plantillas YAML**. Cada plantilla describe qué solicitud o interacción debe realizarse, qué condiciones de respuesta verificar y cómo clasificar el hallazgo. Puede analizar aplicaciones web, servicios de red, DNS, certificados, archivos y otros contextos dependiendo de la plantilla. No equivale a un inventario de parches de un sistema operativo, ni garantiza detectar todas sus vulnerabilidades.

Su proceso general consiste en definir objetivos, seleccionar plantillas o filtros, configurar límites de ejecución, analizar los resultados y validar los hallazgos.

En esta guía los comandos se muestran desde PowerShell y la carpeta que contiene `nuclei.exe`; en Linux se ejecutaría normalmente `nuclei` sin el prefijo `.\`.

## 2. Parámetros completos de la versión 3.11.1

Las tablas siguientes recogen **todas las opciones mostradas en la ayuda adjunta**, incluidas opciones avanzadas, experimentales y obsoletas. Las abreviaturas se muestran junto a sus formas largas cuando existen.

### Opciones comunes (`COMMON`)

| Parámetro          | Función                                                                               |
| ------------------ | ------------------------------------------------------------------------------------- |
| `-mt`, `-max-time` | Duración máxima de ejecución; permite detener automáticamente un análisis prolongado. |

### Definición de objetivos (`TARGET`)

| Parámetro               | Función                                                                                        |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| `-u`, `-target`         | Establece una o varias URL, direcciones o hosts objetivo.                                      |
| `-l`, `-list`           | Lee objetivos desde un archivo, uno por línea.                                                 |
| `-targets-inline`       | Define objetivos multilínea dentro de perfiles de plantillas.                                  |
| `-eh`, `-exclude-hosts` | Excluye hosts, IP o rangos CIDR de la lista.                                                   |
| `-resume`               | Permite reanudar un escaneo desde un archivo de estado; desactiva la agrupación de peticiones. |
| `-sa`, `-scan-all-ips`  | Analiza todas las IP asociadas a los registros DNS de un nombre.                               |
| `-iv`, `-ip-version`    | Selecciona IPv4, IPv6 o ambos para resolver un nombre (por defecto IPv4).                      |

### Formatos de entrada (`TARGET-FORMAT`)

| Parámetro                         | Función                                                                                     |
| --------------------------------- | ------------------------------------------------------------------------------------------- |
| `-im`, `-input-mode`              | Indica cómo interpretar el archivo de entrada: lista, Burp, JSONL, YAML, OpenAPI o Swagger. |
| `-ro`, `-required-only`           | En especificaciones de API, genera solicitudes solo con campos obligatorios.                |
| `-sfv`, `-skip-format-validation` | Omite la validación de formato, por ejemplo variables ausentes.                             |
| `-vtt`, `-vars-text-templating`   | Activa la sustitución de variables en texto cuando la entrada es YAML.                      |
| `-vfp`, `-var-file-paths`         | Carga archivos YAML con variables que se insertarán en la entrada.                          |

### Plantillas y workflows (`TEMPLATES`)

| Parámetro                             | Función                                                                                                                           |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `-nt`, `-new-templates`               | Ejecuta únicamente las plantillas incorporadas en la última publicación.                                                          |
| `-ntv`, `-new-templates-version`      | Selecciona plantillas nuevas de una versión concreta.                                                                             |
| `-as`, `-automatic-scan`              | Detecta tecnologías web y selecciona etiquetas asociadas automáticamente; no equivale a auditar todo el sistema.                  |
| `-t`, `-templates`                    | Indica plantillas o carpetas YAML que se deben ejecutar.                                                                          |
| `-turl`, `-template-url`              | Carga plantillas desde direcciones URL indicadas.                                                                                 |
| `-ai`, `-prompt`                      | Genera y ejecuta una plantilla a partir de instrucciones en lenguaje natural mediante IA; verificar su contenido antes de usarla. |
| `-w`, `-workflows`                    | Ejecuta flujos de trabajo que encadenan plantillas.                                                                               |
| `-wurl`, `-workflow-url`              | Carga flujos de trabajo desde URL.                                                                                                |
| `-validate`                           | Comprueba si las plantillas suministradas son válidas.                                                                            |
| `-nss`, `-no-strict-syntax`           | Desactiva la validación sintáctica estricta.                                                                                      |
| `-td`, `-template-display`            | Muestra el contenido de las plantillas seleccionadas, incluidos metadatos y condiciones.                                          |
| `-tl`                                 | Lista plantillas que cumplen los filtros, sin lanzar un escaneo.                                                                  |
| `-tgl`                                | Lista las etiquetas disponibles.                                                                                                  |
| `-sign`                               | Firma plantillas con la clave privada indicada en la variable de entorno correspondiente.                                         |
| `-code`                               | Habilita plantillas que ejecutan comprobaciones basadas en código; requieren especial precaución.                                 |
| `-dut`, `-disable-unsigned-templates` | Impide ejecutar plantillas sin firma válida o con firma no coincidente.                                                           |
| `-esc`, `-enable-self-contained`      | Permite plantillas autocontenidas que no dependen del objetivo suministrado.                                                      |
| `-egm`, `-enable-global-matchers`     | Habilita plantillas que comparan respuestas globalmente.                                                                          |
| `-file`                               | Habilita plantillas que analizan archivos locales.                                                                                |

### Filtros (`FILTERING`)

| Parámetro                    | Función                                                                         |
| ---------------------------- | ------------------------------------------------------------------------------- |
| `-a`, `-author`              | Filtra por autor de las plantillas.                                             |
| `-tags`                      | Selecciona plantillas con determinadas etiquetas (por ejemplo, tech o apache).  |
| `-etags`, `-exclude-tags`    | Excluye etiquetas determinadas.                                                 |
| `-itags`, `-include-tags`    | Fuerza la inclusión de etiquetas que normalmente quedarían excluidas.           |
| `-id`, `-template-id`        | Selecciona identificadores de plantilla; útil para una CVE específica.          |
| `-eid`, `-exclude-id`        | Excluye identificadores de plantillas.                                          |
| `-it`, `-include-templates`  | Incluye rutas de plantillas incluso si una regla las excluye.                   |
| `-et`, `-exclude-templates`  | Excluye plantillas o directorios concretos.                                     |
| `-em`, `-exclude-matchers`   | Excluye condiciones de coincidencia específicas de los resultados.              |
| `-s`, `-severity`            | Filtra por gravedad: info, low, medium, high, critical o unknown.               |
| `-es`, `-exclude-severity`   | Excluye los niveles de gravedad indicados.                                      |
| `-pt`, `-type`               | Limita plantillas por tipo de protocolo, como HTTP, TCP, DNS, SSL o JavaScript. |
| `-ept`, `-exclude-type`      | Excluye los tipos de protocolo señalados.                                       |
| `-tc`, `-template-condition` | Filtra plantillas mediante expresiones sobre sus atributos.                     |

### Resultados e informes (`OUTPUT`)

| Parámetro                 | Función                                                                    |
| ------------------------- | -------------------------------------------------------------------------- |
| `-o`, `-output`           | Guarda los hallazgos en un archivo.                                        |
| `-sresp`, `-store-resp`   | Guarda las peticiones y respuestas.                                        |
| `-srd`, `-store-resp-dir` | Selecciona la carpeta para guardar peticiones y respuestas.                |
| `-silent`                 | Muestra solo los hallazgos, sin mensajes adicionales.                      |
| `-nc`, `-no-color`        | Elimina colores ANSI de la salida.                                         |
| `-j`, `-jsonl`            | Emite resultados en formato JSONL.                                         |
| `-irr`, `-include-rr`     | Incluye peticiones y respuestas en exportaciones; obsoleta según la ayuda. |
| `-or`, `-omit-raw`        | Omite peticiones y respuestas completas en los informes.                   |
| `-ot`, `-omit-template`   | Omite la plantilla codificada en resultados JSON/JSONL.                    |
| `-nm`, `-no-meta`         | Oculta metadatos de la salida por consola.                                 |
| `-ts`, `-timestamp`       | Añade fecha y hora a los hallazgos.                                        |
| `-rdb`, `-report-db`      | Mantiene resultados en una base de datos de informes.                      |
| `-ms`, `-matcher-status`  | Muestra también estados de coincidencia fallida.                           |
| `-me`, `-markdown-export` | Exporta resultados a un directorio Markdown.                               |
| `-se`, `-sarif-export`    | Exporta resultados en formato SARIF.                                       |
| `-je`, `-json-export`     | Exporta un archivo JSON.                                                   |
| `-jle`, `-jsonl-export`   | Exporta un archivo JSONL.                                                  |
| `-pe`, `-pdf-export`      | Exporta un archivo PDF.                                                    |
| `-rd`, `-redact`          | Oculta claves o campos señalados en parámetros, cabeceras y cuerpos.       |

### Configuración general (`CONFIGURATIONS`)

| Parámetro                                | Función                                                                                      |
| ---------------------------------------- | -------------------------------------------------------------------------------------------- |
| `-config`                                | Carga un archivo de configuración específico de Nuclei.                                      |
| `-tp`, `-profile`                        | Aplica un perfil de plantillas predefinido.                                                  |
| `-tpl`, `-profile-list`                  | Lista los perfiles comunitarios disponibles.                                                 |
| `-fr`, `-follow-redirects`               | Sigue redirecciones HTTP.                                                                    |
| `-fhr`, `-follow-host-redirects`         | Sigue redirecciones solo dentro del mismo host.                                              |
| `-mr`, `-max-redirects`                  | Limita cuántas redirecciones se pueden seguir.                                               |
| `-dr`, `-disable-redirects`              | Desactiva el seguimiento de redirecciones.                                                   |
| `-rc`, `-report-config`                  | Archivo de configuración del módulo de informes.                                             |
| `-H`, `-header`                          | Añade cabeceras HTTP o cookies personalizadas.                                               |
| `-V`, `-var`                             | Define variables personalizadas clave=valor.                                                 |
| `-r`, `-resolvers`                       | Usa una lista de servidores DNS.                                                             |
| `-sr`, `-system-resolvers`               | Utiliza los resolutores DNS del sistema como alternativa.                                    |
| `-dc`, `-disable-clustering`             | Desactiva la agrupación de solicitudes equivalentes.                                         |
| `-passive`                               | Procesa respuestas HTTP de modo pasivo.                                                      |
| `-fh2`, `-force-http2`                   | Fuerza solicitudes HTTP/2.                                                                   |
| `-ev`, `-env-vars`                       | Permite utilizar variables de entorno dentro de las plantillas.                              |
| `-cc`, `-client-cert`                    | Indica certificado cliente para autenticación TLS.                                           |
| `-ck`, `-client-key`                     | Indica la clave privada del certificado cliente.                                             |
| `-ca`, `-client-ca`                      | Indica la autoridad certificadora del cliente.                                               |
| `-sml`, `-show-match-line`               | Muestra líneas coincidentes en plantillas de archivos que emplean extractores.               |
| `-ztls`                                  | Utiliza el motor ztls; opción obsoleta según la ayuda.                                       |
| `-sni`                                   | Establece el nombre SNI para conexiones TLS.                                                 |
| `-dka`, `-dialer-keep-alive`             | Establece la duración de reutilización de conexiones.                                        |
| `-lfa`, `-allow-local-file-access`       | Permite acceso a archivos locales desde cualquier ubicación; revisar alcance por seguridad.  |
| `-lna`, `-restrict-local-network-access` | Impide conexiones a redes locales o privadas.                                                |
| `-i`, `-interface`                       | Selecciona la interfaz de red empleada en escaneos.                                          |
| `-at`, `-attack-type`                    | Elige cómo combinar payloads: batteringram, pitchfork o clusterbomb.                         |
| `-sip`, `-source-ip`                     | Fija una IP origen para conexiones de red.                                                   |
| `-rsr`, `-response-size-read`            | Limita el volumen de respuesta que se lee.                                                   |
| `-rss`, `-response-size-save`            | Limita el volumen de respuesta que se guarda.                                                |
| `-reset`                                 | Restablece la configuración y elimina datos de Nuclei, incluidas las plantillas descargadas. |
| `-tlsi`, `-tls-impersonate`              | Experimenta con variaciones del handshake TLS (JA3).                                         |
| `-hae`, `-http-api-endpoint`             | Habilita un endpoint experimental de API HTTP.                                               |

### Comprobaciones OAST/Interactsh (`INTERACTSH`)

| Parámetro                        | Función                                                                  |
| -------------------------------- | ------------------------------------------------------------------------ |
| `-iserver`, `-interactsh-server` | Indica un servidor Interactsh/OAST alternativo.                          |
| `-itoken`, `-interactsh-token`   | Define el token para autenticarse en Interactsh autohospedado.           |
| `-interactions-cache-size`       | Número máximo de interacciones guardadas temporalmente.                  |
| `-interactions-eviction`         | Plazo de caducidad de interacciones en memoria.                          |
| `-interactions-poll-duration`    | Intervalo entre consultas al servidor OAST.                              |
| `-interactions-cooldown-period`  | Tiempo adicional de espera antes de finalizar las comprobaciones OAST.   |
| `-ni`, `-no-interactsh`          | Desactiva Interactsh y omite las plantillas que dependen de esa técnica. |

### Fuzzing y DAST (`FUZZING`)

| Parámetro                       | Función                                                                                        |
| ------------------------------- | ---------------------------------------------------------------------------------------------- |
| `-ft`, `-fuzzing-type`          | Sobrescribe la forma de insertar entradas de prueba: reemplazo, prefijo, sufijo o intercalado. |
| `-fm`, `-fuzzing-mode`          | Elige si las pruebas usan uno o varios parámetros a la vez.                                    |
| `-fuzz`                         | Habilita las antiguas plantillas fuzzing; opción obsoleta.                                     |
| `-dast`                         | Activa plantillas de análisis dinámico DAST.                                                   |
| `-dts`, `-dast-server`          | Activa el servidor de fuzzing en vivo.                                                         |
| `-dtr`, `-dast-report`          | Guarda un informe de escaneo DAST.                                                             |
| `-dtst`, `-dast-server-token`   | Token opcional del servidor DAST.                                                              |
| `-dtsa`, `-dast-server-address` | Dirección del servidor de fuzzing; por defecto, localhost:9055.                                |
| `-dfp`, `-display-fuzz-points`  | Muestra los puntos de inyección de las pruebas.                                                |
| `-fuzz-param-frequency`         | Controla cuándo omitir parámetros poco interesantes por repetición.                            |
| `-fa`, `-fuzz-aggression`       | Selecciona intensidad del fuzzing: baja, media o alta.                                         |
| `-cs`, `-fuzz-scope`            | Define mediante expresiones regulares las URL incluidas en el alcance.                         |
| `-cos`, `-fuzz-out-scope`       | Excluye del fuzzing URL que coincidan con expresiones regulares.                               |

### Descubrimiento de activos externos (`UNCOVER`)

| Parámetro                   | Función                                                                   |
| --------------------------- | ------------------------------------------------------------------------- |
| `-uc`, `-uncover`           | Activa la búsqueda de activos mediante motores externos.                  |
| `-uq`, `-uncover-query`     | Define la consulta de búsqueda de activos.                                |
| `-ue`, `-uncover-engine`    | Selecciona motor externo, por ejemplo Shodan, Censys u otros compatibles. |
| `-uf`, `-uncover-field`     | Define qué campos de los activos se recuperan: IP, puerto o host.         |
| `-ul`, `-uncover-limit`     | Limita el número de resultados obtenidos.                                 |
| `-ur`, `-uncover-ratelimit` | Modifica el límite de consultas para motores con umbral no conocido.      |

### Velocidad y concurrencia (`RATE-LIMIT`)

| Parámetro                               | Función                                                      |
| --------------------------------------- | ------------------------------------------------------------ |
| `-rl`, `-rate-limit`                    | Máximo de solicitudes por segundo.                           |
| `-rld`, `-rate-limit-duration`          | Intervalo de tiempo al que aplica el límite.                 |
| `-per-host-rate-limit`                  | Limita peticiones por host en lugar del límite global.       |
| `-rlm`, `-rate-limit-minute`            | Límite de solicitudes por minuto; obsoleto.                  |
| `-bs`, `-bulk-size`                     | Número de hosts en paralelo por plantilla.                   |
| `-c`, `-concurrency`                    | Número de plantillas ejecutadas en paralelo.                 |
| `-hbs`, `-headless-bulk-size`           | Número de hosts simultáneos por plantilla con navegador.     |
| `-headc`, `-headless-concurrency`       | Número de plantillas de navegador simultáneas.               |
| `-jsc`, `-js-concurrency`               | Número máximo de runtimes JavaScript simultáneos.            |
| `-pc`, `-payload-concurrency`           | Número máximo de payloads simultáneos por plantilla.         |
| `-prc`, `-probe-concurrency`            | Número de comprobaciones simultáneas de descubrimiento HTTP. |
| `-tlc`, `-template-loading-concurrency` | Número de plantillas cargadas en paralelo.                   |

### Optimización y fiabilidad (`OPTIMIZATIONS`)

| Parámetro                      | Función                                                                                         |
| ------------------------------ | ----------------------------------------------------------------------------------------------- |
| `-timeout`                     | Tiempo máximo de espera por solicitud, en segundos.                                             |
| `-retries`                     | Número de reintentos de una solicitud fallida.                                                  |
| `-ldp`, `-leave-default-ports` | Conserva los puertos HTTP/HTTPS predeterminados en la forma del objetivo.                       |
| `-mhe`, `-max-host-error`      | Número de errores de un host antes de omitirlo.                                                 |
| `-te`, `-track-error`          | Añade clases de errores al contador de fallos de host.                                          |
| `-nmhe`, `-no-mhe`             | Desactiva la lógica de omisión por errores acumulados.                                          |
| `-project`                     | Reutiliza un directorio de proyecto para no repetir solicitudes idénticas.                      |
| `-project-path`                | Selecciona el directorio donde guardar ese proyecto.                                            |
| `-spm`, `-stop-at-first-match` | Detiene peticiones HTTP de una plantilla tras la primera coincidencia; puede alterar su lógica. |
| `-stream`                      | Procesa entradas en flujo continuo, sin ordenación previa.                                      |
| `-ss`, `-scan-strategy`        | Selecciona la estrategia de reparto de escaneos: automática, por host o por plantilla.          |
| `-irt`, `-input-read-timeout`  | Tiempo máximo de lectura de la entrada.                                                         |
| `-nh`, `-no-httpx`             | Deshabilita la detección HTTP automática de objetivos sin esquema URL.                          |
| `-preflight-portscan`          | Ejecuta una comprobación previa de resolución y puertos TCP.                                    |
| `-no-stdin`                    | Evita leer objetivos de la entrada estándar.                                                    |

### Navegador automatizado (`HEADLESS`)

| Parámetro                       | Función                                                      |
| ------------------------------- | ------------------------------------------------------------ |
| `-headless`                     | Habilita plantillas que requieren un navegador automatizado. |
| `-page-timeout`                 | Tiempo máximo para cargar una página.                        |
| `-sb`, `-show-browser`          | Muestra visualmente el navegador durante el análisis.        |
| `-ho`, `-headless-options`      | Pasa opciones adicionales a Chrome.                          |
| `-sc`, `-system-chrome`         | Utiliza Chrome instalado en el sistema.                      |
| `-cdpe`, `-cdp-endpoint`        | Conecta con un navegador remoto mediante CDP.                |
| `-lha`, `-list-headless-action` | Lista las acciones disponibles para plantillas de navegador. |

### Diagnóstico y depuración (`DEBUG`)

| Parámetro                    | Función                                                             |
| ---------------------------- | ------------------------------------------------------------------- |
| `-debug`                     | Muestra solicitudes y respuestas completas; puede revelar secretos. |
| `-dreq`, `-debug-req`        | Muestra únicamente las solicitudes enviadas.                        |
| `-dresp`, `-debug-resp`      | Muestra únicamente las respuestas recibidas.                        |
| `-p`, `-proxy`               | Utiliza un proxy HTTP o SOCKS5.                                     |
| `-pi`, `-proxy-internal`     | Encamina también tráfico interno de Nuclei por el proxy.            |
| `-ldf`, `-list-dsl-function` | Lista funciones disponibles del lenguaje de expresiones DSL.        |
| `-tlog`, `-trace-log`        | Guarda traza de solicitudes enviadas.                               |
| `-elog`, `-error-log`        | Guarda los mensajes de error en un archivo.                         |
| `-version`                   | Muestra la versión del motor.                                       |
| `-hm`, `-hang-monitor`       | Activa la monitorización de bloqueos.                               |
| `-v`, `-verbose`             | Aumenta la información de diagnóstico mostrada.                     |
| `-profile-mem`               | Crea perfiles de memoria y archivos de traza.                       |
| `-vv`                        | Muestra las plantillas que se cargan en el escaneo.                 |
| `-svd`, `-show-var-dump`     | Muestra los valores de variables para depuración.                   |
| `-vdl`, `-var-dump-limit`    | Limita los caracteres mostrados por variable.                       |
| `-ep`, `-enable-pprof`       | Activa el servidor de diagnóstico pprof.                            |
| `-tv`, `-templates-version`  | Muestra la versión instalada de las plantillas.                     |
| `-hc`, `-health-check`       | Realiza una comprobación diagnóstica de salud.                      |

### Actualizaciones (`UPDATE`)

| Parámetro                       | Función                                                    |
| ------------------------------- | ---------------------------------------------------------- |
| `-up`, `-update`                | Actualiza el ejecutable de Nuclei a la última versión.     |
| `-ut`, `-update-templates`      | Descarga o actualiza las plantillas oficiales.             |
| `-ud`, `-update-template-dir`   | Selecciona una carpeta personalizada para las plantillas.  |
| `-duc`, `-disable-update-check` | Deshabilita la comprobación automática de actualizaciones. |

### Detección de honeypots (`HONEYPOT`)

| Parámetro                     | Función                                                                                 |
| ----------------------------- | --------------------------------------------------------------------------------------- |
| `-hpd`, `-honeypot-detect`    | Intenta identificar hosts que parezcan honeypots por concentración de coincidencias.    |
| `-hpt`, `-honeypot-threshold` | Establece el número de plantillas distintas necesarias para marcar un posible honeypot. |
| `-shp`, `-suppress-honeypot`  | Oculta los resultados de hosts marcados como honeypots.                                 |

### Estadísticas (`STATISTICS`)

| Parámetro                | Función                                                               |
| ------------------------ | --------------------------------------------------------------------- |
| `-stats`                 | Muestra estadísticas periódicas del escaneo.                          |
| `-sj`, `-stats-json`     | Emite estadísticas en formato JSONL.                                  |
| `-si`, `-stats-interval` | Establece el intervalo de presentación de estadísticas.               |
| `-mp`, `-metrics-port`   | Publica métricas en un puerto HTTP.                                   |
| `-hps`, `-http-stats`    | Registra códigos de estado HTTP con fines estadísticos; experimental. |

### Integración con Cloud (`CLOUD`)

| Parámetro                   | Función                                                      |
| --------------------------- | ------------------------------------------------------------ |
| `-auth`                     | Configura credenciales de ProjectDiscovery Cloud.            |
| `-tid`, `-team-id`          | Indica el identificador del equipo de nube.                  |
| `-cup`, `-cloud-upload`     | Sube resultados a la nube; opción obsoleta.                  |
| `-sid`, `-scan-id`          | Asocia resultados con un identificador de escaneo existente. |
| `-sname`, `-scan-name`      | Asigna un nombre al escaneo.                                 |
| `-pd`, `-dashboard`         | Sube o consulta resultados en el panel Cloud.                |
| `-pdu`, `-dashboard-upload` | Sube un archivo JSONL de resultados al panel de la nube.     |

### Autenticación (`AUTHENTICATION`)

| Parámetro                  | Función                                                                                               |
| -------------------------- | ----------------------------------------------------------------------------------------------------- |
| `-sf`, `-secret-file`      | Utiliza un archivo de secretos para escaneos autenticados, según protocolos y plantillas compatibles. |
| `-ps`, `-prefetch-secrets` | Precarga secretos desde el archivo antes de ejecutar las comprobaciones.                              |

## 3. Tipos de escaneo habituales

Las siguientes modalidades son **usos prácticos habituales**, no categorías cerradas del motor. La selección de plantillas depende del contexto del objetivo. Solo se deben analizar sistemas propios o expresamente autorizados.

### 3.1. Detección de tecnologías

Permite identificar tecnologías conocidas en un servicio web. Las coincidencias de nivel `info` no implican necesariamente una vulnerabilidad.

```powershell
.\nuclei.exe -u http://192.168.56.103 -tags tech -rl 10 -ni
```

### 3.2. Verificación de una CVE concreta

Ejecuta una plantilla específica. Es importante indicar una ruta o servicio en el que las condiciones de la comprobación puedan verificarse; una URL demasiado genérica puede provocar falsos negativos.

```powershell
.\nuclei.exe -u http://192.168.56.103:3000/admin -id CVE-2025-29927 -ni
```

### 3.3. Análisis por gravedad

Selecciona las plantillas clasificadas como altas o críticas. La gravedad procede de los metadatos de las plantillas, no de una evaluación individualizada del riesgo. Incluso con el filtro, el número de comprobaciones puede ser elevado.

```powershell
.\nuclei.exe -u http://192.168.56.103 -s high,critical -rl 5 -c 2 -ni
```

### 3.4. Detección de configuraciones inseguras

Busca ajustes deficientes y cabeceras HTTP ausentes, entre otros indicadores. Una coincidencia no siempre equivale a una vulnerabilidad explotable y debe interpretarse según el contexto.

```powershell
.\nuclei.exe -u http://192.168.56.103:3000 -t http/misconfiguration/ -rl 10 -c 3 -ni
```

### 3.5. Exposición de información

Comprueba rutas, archivos y configuraciones que podrían ser accesibles indebidamente. Esta familia incluye numerosas plantillas y puede generar muchas peticiones.

```powershell
.\nuclei.exe -u http://192.168.56.103 -t http/exposures/ -rl 10 -c 3 -ni
```

### 3.6. Selección de plantillas por tecnología

Restringe las pruebas a plantillas etiquetadas para un producto concreto. Por ejemplo, `apache` no representa una auditoría exhaustiva de todas las vulnerabilidades del servidor Apache.

```powershell
.\nuclei.exe -u http://192.168.56.103 -tags apache -ni
```

### 3.7. Escaneo automático de aplicaciones web

Utiliza la detección de tecnologías para asociarlas a etiquetas y escoger plantillas relacionadas. Conviene revisar el alcance y las peticiones previstas; la selección automática no garantiza un análisis completo ni necesariamente rápido.

```powershell
.\nuclei.exe -u http://192.168.56.103 -as -rl 5 -c 2 -ni
```

### 3.8. Análisis de servicios de red

Las plantillas de red permiten detectar servicios y comprobar ciertas propiedades de protocolos como SSH. Una plantilla de identificación de OpenSSH informa del servicio, pero no confirma una vulnerabilidad. Para auditar SSH deben elegirse comprobaciones específicas.

```powershell
.\nuclei.exe -u 192.168.56.103:22 -t network/detection/openssh-detect.yaml -ni
```

### 3.9. Análisis de varios objetivos

Permite automatizar comprobaciones sobre un conjunto de direcciones o servicios definidos en un archivo, con un objetivo por línea. Se recomienda mantener la lista dentro del alcance expresamente autorizado.

```powershell
.\nuclei.exe -l .\objetivos.txt -tags tech -rl 5 -ni
```

### 3.10. Escaneo con generación de informe

Además de mostrar hallazgos en consola, Nuclei puede exportarlos a Markdown para su revisión y documentación. Antes de publicarlos se deben comprobar las evidencias y retirar posibles datos sensibles.

```powershell
.\nuclei.exe -u http://192.168.56.103:8001 -id CVE-2020-11710 -ni -me .\Informes\Kong
```

### Variantes avanzadas

El fuzzing (`-dast`) y las plantillas `headless`, `code` o que generan payloads no deben activarse indiscriminadamente. También pueden emplearse flujos de trabajo (`-w`), perfiles (`-tp`), entradas OpenAPI (`-im openapi`), plantillas propias y análisis autenticados con archivos de secretos (`-sf`). Antes de utilizarlos hay que comprobar impacto, autenticación, alcance y permisos.

## 4. Lectura e interpretación de los resultados

Un hallazgo suele mostrar el **identificador de plantilla**, el **protocolo**, la **gravedad** y el **objetivo afectado**. Puede añadir detalles extraídos de la respuesta. Por ejemplo:

```text
[CVE-2025-29927] [http] [critical] http://192.168.56.103:3000/admin
```

Las etiquetas `info`, `low`, `medium`, `high`, `critical` y `unknown` clasifican la comprobación o el hallazgo. No sustituyen una evaluación contextual del riesgo. Si aparecen varias coincidencias de una misma CVE pueden ser condiciones distintas de la misma plantilla, no vulnerabilidades independientes.

Un resultado **sin coincidencias** no certifica que el activo esté libre de vulnerabilidades. Puede faltar una plantilla, una ruta protegida, autenticación, compatibilidad con el protocolo o condiciones específicas. Asimismo, una coincidencia puede reflejar una exposición o configuración y no la aplicabilidad exacta de una CVE al producto instalado.

Para investigar una detección se recomienda consultar `-td`, revisar la evidencia con `-debug` , verificar versiones y configuraciones y, si procede, contrastar manualmente la respuesta del servicio.

## 5. Informes, actualización y mantenimiento

```powershell
.\nuclei.exe -version
.\nuclei.exe -tv
.\nuclei.exe -update-templates
.\nuclei.exe -tags ssh -tl
.\nuclei.exe -id CVE-2025-29927 -td
```

Los informes se pueden guardar en Markdown (`-me`), JSON (`-je`), JSONL (`-jle`), SARIF (`-se`) y PDF (`-pe`). Para integraciones automatizadas, JSONL es especialmente cómodo; para el repositorio de la práctica, Markdown permite consultar los resultados sin herramientas adicionales. Revisar siempre si las exportaciones contienen tokens, cookies, rutas o información interna.

## 6. Recomendaciones de uso

1. **Autorizar y acotar el análisis.** Definir IP, puertos, URL, rutas y técnicas permitidas. Restringir el laboratorio vulnerable a la red de pruebas.
2. **Empezar con pocas plantillas.** La ejecución de miles de comprobaciones contra una sola página puede ser innecesaria. Preferir tecnologías y familias relevantes.
3. **Regular carga y concurrencia.** Utilizar `-rl`, `-c`, `-timeout` y, cuando haga falta, `-mt`. Los límites de tasa no impiden que cargar plantillas consuma CPU.
4. **Mantener motor y plantillas actualizados.** Una plantilla puede variar sus comprobaciones entre versiones; registrar versión en los informes.
5. **Validar las coincidencias.** Revisar qué comprueba la plantilla, las versiones afectadas, el contexto y los resultados HTTP. Distinguir error de configuración, CVE y hallazgo informativo.
6. **Revisar los negativos.** Si una CVE conocida no aparece, comprobar la ruta, la autenticación, la plantilla y los posibles errores de red antes de darla por descartada.
7. **Evitar pruebas disruptivas por defecto.** Revisar especialmente fuzzing, payloads de escritura, plantillas de código y verificaciones de denegación de servicio.
8. **No publicar evidencias sensibles.** Los informes y las opciones de depuración pueden incluir respuestas, claves o cabeceras privadas. Anonimizar lo necesario antes de subirlos a GitHub.
9. **Repetir las comprobaciones tras remediar.** Comparar resultados iniciales y posteriores ayuda a documentar el cierre de vulnerabilidades.
10. **Complementar con otras herramientas.** Nmap es útil para descubrir puertos; Nessus/OpenVAS para auditar sistemas y parches; Nuclei destaca en comprobaciones reproducibles basadas en plantillas.

## 7. Referencias

- [ProjectDiscovery: ejecución y opciones de Nuclei](https://docs.projectdiscovery.io/tools/nuclei/running)
- [ProjectDiscovery: formatos de entrada](https://docs.projectdiscovery.io/tools/nuclei/input-formats)
- [ProjectDiscovery: catálogo de plantillas](https://github.com/projectdiscovery/nuclei-templates)
- [ProjectDiscovery: repositorio de Nuclei](https://github.com/projectdiscovery/nuclei)
