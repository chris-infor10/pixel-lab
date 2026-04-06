## Nombre stack:
- cloudsecure-config

## ¿Qué es AWS Config?
AWS Config es el servicio de **cumplimiento y evaluación de configuraciones** de AWS. Permite definir reglas que validan automáticamente si los recursos de la cuenta cumplen con las políticas de seguridad establecidas. Si un recurso incumple, Config lo marca como **NON_COMPLIANT**.

## Regla creada:
| Parámetro        | Valor                            |
| ---------------- | -------------------------------- |
| Nombre de regla  | s3-bucket-public-read-prohibited |
| Tipo             | Regla administrada por AWS       |
| Evalúa           | Todos los buckets S3 de la cuenta |
| Estado esperado  | COMPLIANT                        |

## ¿Qué verifica esta regla?
```
s3-bucket-public-read-prohibited verifica que:
  ✔ Ningún bucket S3 tenga acceso público de lectura habilitado
  ✔ Si un bucket tiene ACL pública → NON_COMPLIANT (alerta)
  ✔ Si todos los buckets están privados → COMPLIANT (correcto)
```

## Crearemos lo siguiente:
| Recurso        | Detalle                                              |
| -------------- | ---------------------------------------------------- |
| Config Rule    | Regla administrada: s3-bucket-public-read-prohibited |

> ⚠️ Nota importante: El **ConfigurationRecorder** fue activado manualmente en la consola AWS antes de desplegar el stack. CloudFormation solo crea la regla, no el recorder, para evitar conflictos con AWS Academy.

## Pasos realizados (click por click):

### Activar Config manualmente (prerequisito)
1. Ir a **AWS Config** en la consola AWS
2. Click → **Get started** (si es la primera vez)
3. En **Settings**: seleccionar **Record all resources**
4. Asignar un bucket S3 para almacenar el historial de configuraciones
5. Click → **Confirm**

### Verificar la regla desplegada
1. Ir a **AWS Config**
2. Click en **Rules**
3. Deberías ver: `s3-bucket-public-read-prohibited`
4. Estado: **COMPLIANT** (si el bucket seguro está correctamente configurado)

### ¿Qué significa cada estado?
| Estado        | Significado                                              |
| ------------- | -------------------------------------------------------- |
| COMPLIANT     | El recurso cumple la política de seguridad ✔            |
| NON_COMPLIANT | El recurso viola la política → requiere corrección ✘    |
| NOT_APPLICABLE | La regla no aplica a ese tipo de recurso               |

## Verificar
- Ir a **AWS Config**
- Entrar a: **Rules**
- Deberías ver: `s3-bucket-public-read-prohibited` con estado **COMPLIANT** ✔
- Click en la regla → ver qué recursos evaluó y su resultado individual

## ¿Por qué es importante para Blue Wave?
AWS Config permite validar **automáticamente** que ningún bucket de la fintech sea accesible públicamente. Si alguien accidentalmente abre un bucket, Config lo detecta de inmediato y cambia el estado a NON_COMPLIANT, permitiendo corregirlo antes de que ocurra una filtración.
