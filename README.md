# Fundamentos de Arquitectura Cloud - Laboratorios

Este repositorio contiene el desarrollo de los proyectos de laboratorio para el curso **Fundamentos de Arquitectura Cloud** (Academia Pixelab). Cada proyecto responde a un caso de negocio simulado para aplicar conceptos de migración, arquitectura desacoplada, soluciones sin servidor y seguridad en la nube pública.

## Estructura del Repositorio
El repositorio está compuesto por 4 carpetas principales, las cuales corresponden a los desafíos planteados en cada módulo:

### 1. Módulo 4: Infraestructura Viva (`[carpeta_modulo_04]`)
Este proyecto aborda la propuesta de modernización y migración de la infraestructura local (*on-premise*) a la nube de la empresa "Soluciones Digitales ACME".
* **Objetivo**: Desarrollar una propuesta de migración utilizando herramientas gratuitas y recursos de AWS Academy.
* **Requerimientos clave**:
  * Planteamiento de despliegue de aplicaciones mediante máquinas virtuales, contenedores o funciones serverless.
  * Integración de una base de datos relacional gestionada (como RDS) y una solución NoSQL (como DynamoDB).
  * Configuración de almacenamiento de objetos (Amazon S3) con políticas de ciclo de vida para derivar a Glacier.
  * Configuración de una red virtual (VPC) con subredes públicas y privadas, balanceador de carga y reglas de seguridad.
  * Implementación de monitoreo con CloudWatch y sistemas de notificación mediante SNS/SQS.
* **Herramientas utilizadas**: Visual Studio Code, SQLiteOnline y recursos de AWS Academy.

### 2. Módulo 7: Microservicios Orquestados (`[carpeta_modulo_07]`)
En este laboratorio se trabajó en desacoplar el sistema monolítico de la fintech "MicroPay" para mitigar cuellos de botella y mejorar la agilidad en lanzamientos.
* **Objetivo**: Diseñar e implementar una arquitectura basada en microservicios aplicando buenas prácticas de escalabilidad, disponibilidad y desacoplamiento en un entorno educativo.
* **Requerimientos clave**:
  * Creación y contenerización de al menos 2 microservicios básicos utilizando Docker.
  * Carga y alojamiento de las imágenes de contenedores en Amazon ECR.
  * Despliegue de los microservicios mediante tareas en Amazon ECS con el tipo de lanzamiento AWS Fargate.
  * Configuración de un punto de entrada centralizado a través de Amazon API Gateway con rutas diferenciadas.
  * Centralización de logs para la auditoría y monitoreo básico con CloudWatch Logs.

### 3. Módulo 8: Serverless Inteligente (`[carpeta_modulo_08]`)
Caso enfocado en reducir costos de inactividad de una compañía de comercio electrónico a través de la transición a una arquitectura 100% libre de administración de servidores.
* **Objetivo**: Diseñar e implementar una aplicación serverless en AWS Academy, mostrando cómo escalar sin administrar servidores.
* **Requerimientos clave**:
  * Creación de al menos 2 funciones de cómputo en AWS Lambda con Python o Node.js.
  * Exposición de funciones Lambda como endpoints a través de Amazon API Gateway.
  * Persistencia de datos operando sobre una tabla NoSQL de Amazon DynamoDB a través de las funciones Lambda.
  * Alojamiento de un sitio estático web (landing page) en un bucket público en Amazon S3.
  * Validación e inspección de ejecuciones e invocaciones en CloudWatch Logs.

### 4. Módulo 9: Cloud Secure (`[carpeta_modulo_09]`)
Simulación de seguridad para la fintech "Blue Wave", mitigando hallazgos de configuraciones débiles y carencia de monitoreo de auditoría tras su migración.
* **Objetivo**: Aplicar principios de seguridad cloud en AWS Academy, priorizando la protección de datos, el monitoreo y cumplimiento.
* **Requerimientos clave**:
  * Creación de un bucket en Amazon S3 implementando cifrado SSE y el bloqueo de acceso público (Block Public Access).
  * Activación de AWS CloudTrail en la región activa para auditar eventos almacenando los logs de rastro en S3.
  * Configuración de AWS Config para evaluar y validar el cumplimiento normativo interno de los recursos desplegados.
  * Parametrización de alertas mediante una alarma preventiva en Amazon CloudWatch.

---
## Consideraciones Técnicas
Las implementaciones documentadas o codificadas en cada carpeta fueron ejecutadas aprovechando las herramientas de capa gratuita y los recursos del laboratorio educativo **AWS Academy Learner Lab**.