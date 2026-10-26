# Análisis de vulnerabilidades con Nuclei

Guía práctica de instalación, funcionamiento y uso de **Nuclei**, el escáner de seguridad basado en plantillas YAML desarrollado por [ProjectDiscovery](https://github.com/projectdiscovery/nuclei).

Este proyecto forma parte de la actividad **«El Arsenal de un Analista»** del módulo de Análisis de Vulnerabilidades. Su objetivo es documentar un flujo de trabajo reproducible: preparar el entorno, seleccionar comprobaciones, analizar hallazgos y contrastar los resultados del escáner.

## Entorno utilizado

| Componente             | Configuración                                               |
| ---------------------- | ----------------------------------------------------------- |
| Equipo de análisis     | Windows 10 con Nuclei **v3.11.1**                           |
| Equipo objetivo        | Ubuntu 22.04 en una máquina virtual                         |
| Red de laboratorio     | Red privada de VirtualBox (`192.168.56.0/24`)               |
| Aplicaciones de prueba | Apache, SSH y laboratorios Docker de Next.js y Kong Gateway |
| Plantillas             | Colección oficial `nuclei-templates` de ProjectDiscovery    |

## Documentación

- **[Instalación](Instalacion.md):** descarga de Nuclei en Windows, verificación del ejecutable e instalación automática o manual de las plantillas.
- **[Funcionamiento](Funcionamiento.md):** explicación de las opciones de la ayuda (`-h`), tipos de análisis, exportación de resultados y recomendaciones de uso.
- **[Ejemplos prácticos](Ejemplos%20practicos.md):** preparación del laboratorio en Ubuntu, análisis efectuados, detecciones, validación e interpretación de resultados.

## Pruebas realizadas

| Caso                         | Resultado principal                                                   |
| ---------------------------- | --------------------------------------------------------------------- |
| Reconocimiento web           | Identificación de Apache en Ubuntu                                    |
| Configuraciones HTTP         | Avisos informativos sobre cabeceras de seguridad ausentes             |
| Reconocimiento SSH           | Identificación de OpenSSH; sin vulnerabilidad confirmada              |
| **CVE-2025-29927 — Next.js** | Evasión del middleware de autorización **confirmada** mediante Nuclei |
| **CVE-2020-11710 — Kong**    | Detección de una **API de administración expuesta**                   |

## Informes de Nuclei

Se incluyen los informes Markdown exportados durante las pruebas:

- [Informe CVE-2025-29927 — Next.js](Informes/CVE-2025-29927.md)
- [Informe CVE-2020-11710 — Kong](Informes/CVE-2020-11710.md)

## Recursos

- [Nuclei — repositorio oficial](https://github.com/projectdiscovery/nuclei)
- [Documentación de Nuclei](https://docs.projectdiscovery.io/tools/nuclei/overview)
- [Plantillas oficiales](https://github.com/projectdiscovery/nuclei-templates)
- [Laboratorios de prueba](https://github.com/projectdiscovery/nuclei-templates-labs)
