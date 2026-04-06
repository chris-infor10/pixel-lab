## Nombre stack:
- cloudsecure-cloudtrail

## ¿Qué es CloudTrail?
AWS CloudTrail es el servicio de **auditoría y gobierno** de AWS. Registra todas las acciones realizadas sobre los recursos de la cuenta: quién hizo qué, desde dónde y cuándo. Es fundamental para cumplimiento normativo y detección de amenazas.

## Recursos creados:
| Recurso          | Nombre                                        | Detalle                          |
| ---------------- | --------------------------------------------- | -------------------------------- |
| Trail            | CloudTrail-us-east-1                          | Registro multi-región activo     |
| Bucket de logs   | cloudtrail-logs-\<accountId\>-\<region\>      | Almacena los eventos auditados   |
| Cifrado          | SSE-S3 (AES-256)                              | Logs cifrados en reposo          |

## Crearemos lo siguiente:
| Recurso              | Detalle                                           |
| -------------------- | ------------------------------------------------- |
| S3 Bucket (Trail)    | Bucket exclusivo para almacenar logs de CloudTrail |
| Bucket Policy        | Permite que CloudTrail escriba en el bucket       |
| CloudTrail Trail     | Trail activo, multi-región, con eventos globales  |

## ¿Qué registra CloudTrail?
```
Ejemplos de eventos auditados:
  ✔ Creación / eliminación de recursos (EC2, S3, Lambda...)
  ✔ Cambios en políticas IAM
  ✔ Accesos a la consola AWS (login/logout)
  ✔ Llamadas a la API de cualquier servicio
  ✔ Modificaciones en Security Groups
```

## Configuración del Trail:
| Parámetro                  | Valor    |
| -------------------------- | -------- |
| IsLogging                  | true     |
| IncludeGlobalServiceEvents | true     |
| IsMultiRegionTrail         | true     |
| Destino de logs            | S3 bucket cifrado |

## Pasos realizados (click por click):

### Verificar CloudTrail activo
1. Ir a **CloudTrail** en la consola AWS
2. Click en **Trails**
3. Deberías ver: `CloudTrail-us-east-1` con estado **Logging: ON**
4. Click en el Trail para ver detalles
5. Verificar que **S3 bucket** apunta al bucket `cloudtrail-logs-...`

### Ver los logs generados
1. Ir a **S3** → bucket `cloudtrail-logs-...`
2. Navegar por la ruta: `AWSLogs/<accountId>/CloudTrail/<region>/<año>/<mes>/<día>/`
3. Verás archivos `.json.gz` con los eventos registrados

### Ver eventos en la consola
1. Ir a **CloudTrail → Event history**
2. Filtrar por usuario, servicio o tipo de evento
3. Ver el historial de acciones recientes en la cuenta

## Verificar
- Ir a **AWS CloudTrail**
- Entrar a: **Trails**
- Deberías ver: `CloudTrail-us-east-1` con:
  - Status: **Logging** ✔
  - Multi-region: **Yes** ✔
  - S3 bucket: `cloudtrail-logs-...` ✔

## ¿Por qué es importante para Blue Wave?
CloudTrail permite detectar accesos no autorizados, cambios inesperados en la infraestructura y cumplir con normativas de auditoría financiera. Cada acción queda registrada con usuario, timestamp e IP de origen.
