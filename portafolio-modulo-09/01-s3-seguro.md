## Nombre stack:
- cloudsecure-s3

## ¿Qué es este componente?
Implementación del **Principio de Mínimo Privilegio** sobre Amazon S3. Se configura un bucket con cifrado habilitado y bloqueo total de acceso público, garantizando que solo identidades explícitamente autorizadas puedan acceder a los datos de la fintech Blue Wave.

## Bucket creado:
| Propiedad              | Valor                                         |
| ---------------------- | --------------------------------------------- |
| Nombre bucket          | cloudsecure-\<accountId\>-\<region\>          |
| Cifrado                | SSE-S3 (AES-256)                              |
| Acceso público         | Bloqueado (Block Public Access activado)      |
| Transporte inseguro    | Denegado (solo HTTPS permitido)               |

## Crearemos lo siguiente:
| Recurso         | Detalle                                          |
| --------------- | ------------------------------------------------ |
| S3 Bucket       | Con cifrado AES-256 habilitado por defecto       |
| Bucket Policy   | Deniega cualquier solicitud sin HTTPS (HTTP denegado) |

## Principio de Mínimo Privilegio aplicado:
```
Sin este control:
  Cualquier usuario → puede leer/escribir en el bucket (riesgo de filtración)

Con este control:
  Usuario no autorizado → ACCESO DENEGADO
  Usuario con HTTPS + credenciales → ACCESO PERMITIDO
```

## Pasos realizados (click por click):

### 1. Crear el bucket (vía CloudFormation)
El bucket se crea automáticamente con el stack `security.yml`.

### 2. Verificar Block Public Access
1. Ir a **S3** en la consola AWS
2. Click en el bucket `cloudsecure-...`
3. Ir a la pestaña **Permissions**
4. Verificar que **Block public access** está completamente activado (4 opciones en ON)

### 3. Verificar cifrado
1. En el mismo bucket, ir a la pestaña **Properties**
2. Bajar hasta **Default encryption**
3. Debe mostrar: **SSE-S3 (AES-256)**

### 4. Verificar Bucket Policy (solo HTTPS)
1. En la pestaña **Permissions → Bucket policy**
2. Verificar que existe la condición `aws:SecureTransport: false` con `Effect: Deny`
3. Esto bloquea cualquier solicitud HTTP (sin cifrado en tránsito)

## Verificar
- Ir a **Amazon S3**
- Entrar al bucket `cloudsecure-...`
- Pestaña **Permissions**: Block public access = **On** ✔
- Pestaña **Properties**: Default encryption = **SSE-S3** ✔
- Pestaña **Permissions → Bucket policy**: política de deny HTTP presente ✔

## ¿Por qué es importante para Blue Wave?
Al activar el Block Public Access y cifrado, se garantiza que, por defecto, **nadie externo tiene acceso a la información sensible** de la fintech, permitiendo el flujo de datos solo a identidades explícitamente autorizadas. Esto evita filtraciones de datos y accesos no autorizados.
