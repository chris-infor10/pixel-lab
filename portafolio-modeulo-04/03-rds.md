## Nombre stack:
- infra-viva-rds

## ¿Qué es este componente?
Amazon RDS (Relational Database Service) es el servicio de base de datos relacional administrado de AWS. Para **Soluciones Digitales ACME**, RDS con motor MySQL gestiona la información estructurada del negocio: usuarios, pedidos, transacciones y datos empresariales, sin que el equipo deba administrar el servidor de base de datos.

## Base de datos creada:
| Parámetro            | Valor              |
| -------------------- | ------------------ |
| Identificador        | viva-db            |
| Motor                | MySQL              |
| Clase de instancia   | db.t3.micro        |
| Almacenamiento       | 20 GB              |
| Usuario admin        | admin              |
| Acceso público       | No (privado)       |
| Retención de backups | 7 días             |
| Subredes             | PrivateSubnetA y B |

## ¿Por qué RDS y no una base de datos en EC2?
| Aspecto           | RDS (administrado)                   | MySQL en EC2 (manual)           |
| ----------------- | ------------------------------------ | ------------------------------- |
| Backups           | Automáticos (retención configurable) | Manuales                        |
| Actualizaciones   | AWS las aplica                       | El equipo debe aplicarlas       |
| Alta disponibilidad | Multi-AZ con un clic               | Configuración compleja          |
| Monitoreo         | Integrado con CloudWatch             | Manual                          |

Para ACME, RDS elimina la carga operativa de administrar la base de datos.

## ¿Qué datos gestiona para ACME?
- Información de usuarios registrados
- Pedidos y transacciones
- Datos estructurados de la aplicación
- Historial de operaciones empresariales

## Crearemos lo siguiente:
| Recurso          | Detalle                                          |
| ---------------- | ------------------------------------------------ |
| DB Security Group | Permite acceso MySQL (puerto 3306) desde la VPC |
| DB Subnet Group  | Agrupa las subredes privadas para RDS            |
| Instancia RDS    | MySQL db.t3.micro en subredes privadas           |

## Prerequisito: obtener IDs de red
Antes de desplegar este stack, necesitas los IDs de la VPC y las subredes privadas creadas en el stack 01-network:

1. Ir a **Amazon VPC → Your VPCs** → copiar **VPC ID** (ej: vpc-0f00bbb098b3d63a5)
2. Ir a **Subnets** → copiar IDs de **PrivateSubnetA** y **PrivateSubnetB**
3. Reemplazar en el archivo `03-rds.yml`:
   ```yaml
   VpcId: vpc-XXXXXXXX        ← reemplazar con tu VPC ID
   SubnetIds:
     - subnet-XXXXXXXX        ← PrivateSubnetA
     - subnet-XXXXXXXX        ← PrivateSubnetB
   ```

## Credenciales de la base de datos:
| Campo    | Valor       |
| -------- | ----------- |
| Usuario  | admin       |
| Password | Admin12345! |

> ⚠️ En producción nunca hardcodear credenciales. Se recomienda usar AWS Secrets Manager.

## Seguridad aplicada:
- La instancia RDS está en **subredes privadas** → no accesible desde internet
- El Security Group solo permite puerto **3306 (MySQL)** desde la VPC interna
- `PubliclyAccessible: false` asegura que no tenga IP pública asignada

## Verificar
- Ir a **Amazon RDS**
- Entrar a: **Databases**
- Deberías ver: **viva-db** con estado **Available** ✔
- Verificar que el endpoint sea privado (no accesible desde internet)
