## Nombre stack:
- cloud-secure

## ¿Qué es CloudFormation?
AWS CloudFormation es el servicio de **Infraestructura como Código (IaC)** de AWS. Permite definir y provisionar todos los recursos de seguridad en un solo archivo YAML, garantizando que los controles de seguridad sean reproducibles, auditables y versionables.

## Stack principal del proyecto:
El archivo `security.yml` despliega **toda la infraestructura de seguridad de Cloud Secure** en un único stack:

| Recurso                  | Nombre lógico       | Servicio     | Lección    |
| ------------------------ | ------------------- | ------------ | ---------- |
| Bucket S3 seguro         | SecureS3Bucket      | S3           | Lección 1  |
| Política bucket seguro   | SecureBucketPolicy  | S3           | Lección 1  |
| Bucket logs CloudTrail   | TrailBucket         | S3           | Lección 2  |
| Política bucket trail    | TrailBucketPolicy   | S3           | Lección 2  |
| Trail de auditoría       | CloudTrail          | CloudTrail   | Lección 2  |
| Regla de cumplimiento    | ConfigRule          | AWS Config   | Lección 3  |
| Alarma de CPU            | CPUAlarm            | CloudWatch   | Lección 4  |

## Pilar de seguridad cubierto por cada recurso:
| Pilar                    | Servicio AWS    | ¿Qué protege?                            |
| ------------------------ | --------------- | ---------------------------------------- |
| Protección de datos      | S3 + Cifrado    | Datos en reposo y en tránsito            |
| Auditoría de eventos     | CloudTrail      | Registro de acciones en la cuenta        |
| Cumplimiento             | AWS Config      | Validación automática de configuraciones |
| Monitoreo activo         | CloudWatch      | Detección de comportamientos anómalos    |

## Pasos realizados (click por click):

### Prerequisito: Activar AWS Config manualmente
Antes de desplegar el stack, activar Config en la consola:
1. Ir a **AWS Config → Get started**
2. Seleccionar **Record all resources**
3. Asignar bucket S3 para historial
4. Confirmar activación

### Desplegar el stack
1. Ir a **CloudFormation** en la consola de AWS
2. Click → **Create stack**
3. Seleccionar → **Upload a template file**
4. Subir el archivo `security.yml`
5. Click → **Next**
6. Ingresar nombre del stack: `cloud-secure`
7. Click → **Next → Next → Create stack**
8. Esperar que todos los recursos pasen a estado **CREATE_COMPLETE**

## Verificar
- Ir a **AWS CloudFormation**
- Entrar a: **Stacks**
- Deberías ver: `cloud-secure` con estado **CREATE_COMPLETE** ✔
- En la pestaña **Resources**: los 7 recursos listados
- En la pestaña **Events**: historial de creación de cada recurso

## Comparación con proyectos anteriores:
| Aspecto          | Proyecto 07 (MicroPay) | Proyecto 08 (Serverless) | Proyecto 09 (Cloud Secure) |
| ---------------- | ---------------------- | ------------------------ | -------------------------- |
| Foco principal   | Microservicios         | Arquitectura serverless  | Seguridad cloud            |
| Compute          | ECS Fargate            | Lambda                   | No aplica                  |
| Base de datos    | No incluida            | DynamoDB                 | No aplica                  |
| Seguridad        | Security Group básico  | IAM Role                 | S3 + CloudTrail + Config + CloudWatch |
| Auditoría        | No incluida            | No incluida              | CloudTrail completo        |
| Cumplimiento     | No incluido            | No incluido              | AWS Config + reglas        |
