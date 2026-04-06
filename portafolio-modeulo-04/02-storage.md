## Nombre stack:
- infra-viva-storage

## ¿Qué es este componente?
Amazon S3 (Simple Storage Service) es el servicio de almacenamiento de objetos de AWS. Para **Soluciones Digitales ACME**, S3 actúa como repositorio centralizado de archivos de la aplicación y sistema de respaldo automatizado con política de ciclo de vida hacia Glacier.

## Buckets creados:
| Bucket                                       | Uso principal                                |
| -------------------------------------------- | -------------------------------------------- |
| infraestructura-viva-app-christian-cortez     | Archivos activos de la aplicación            |
| infraestructura-viva-backup-christian-cortez  | Respaldo de datos y archivos históricos      |

## ¿Qué almacena cada bucket para ACME?

### Bucket principal (app):
- Documentos subidos por usuarios
- Archivos multimedia (imágenes, videos)
- Exportaciones de datos de la aplicación
- Registros de actividad (logs)

### Bucket de backup:
- Copias de seguridad periódicas
- Archivos históricos que ya no se acceden frecuentemente
- Respaldos de la base de datos RDS

## Crearemos lo siguiente:
| Recurso          | Detalle                                           |
| ---------------- | ------------------------------------------------- |
| Bucket principal | Con política de ciclo de vida activa              |
| Bucket backup    | Almacenamiento simple de respaldo                 |
| Lifecycle policy | Mueve datos a Glacier a los 30 días, expira a 365 |

## Política de ciclo de vida (Lifecycle) del bucket principal:
```
Día 0    → Archivo subido al bucket (S3 Standard)
    │
Día 30   → Migración automática a S3 Glacier (costo ~90% menor)
    │
Día 365  → Eliminación automática del archivo
```

Esto reduce costos significativamente para ACME: los archivos antiguos que rara vez se consultan se archivan automáticamente en Glacier sin intervención manual.

## Comparación de costos S3 vs Glacier:
| Clase de almacenamiento | Costo aprox.       | Ideal para                     |
| ----------------------- | ------------------ | ------------------------------ |
| S3 Standard             | ~$0.023 / GB / mes | Archivos de acceso frecuente   |
| S3 Glacier              | ~$0.004 / GB / mes | Archivos de acceso infrecuente |

## Verificar
- Ir a **Amazon S3**
- Entrar a: **Buckets**
- Deberías ver:
  - `infraestructura-viva-app-christian-cortez` ✔
  - `infraestructura-viva-backup-christian-cortez` ✔
- Click en el bucket app → pestaña **Management** → ver la regla **MoveToGlacier** activa
