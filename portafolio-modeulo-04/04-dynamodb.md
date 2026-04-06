## Nombre stack:
- infra-viva-dynamodb

## ¿Qué es este componente?
Amazon DynamoDB es la base de datos NoSQL serverless de AWS. Para **Soluciones Digitales ACME**, DynamoDB complementa a RDS almacenando datos de alta velocidad y acceso frecuente que no requieren un esquema rígido: sesiones de usuario, logs de actividad y datos en tiempo real de la aplicación.

## Tabla creada:
| Parámetro      | Valor          |
| -------------- | -------------- |
| Nombre tabla   | VivaUsers      |
| Clave primaria | UserID (String)|
| Modo de pago   | PAY_PER_REQUEST (On-Demand) |
| Tipo           | NoSQL clave-valor |

## ¿Cuándo usar DynamoDB vs RDS para ACME?
| Criterio              | RDS (MySQL)                        | DynamoDB                             |
| --------------------- | ---------------------------------- | ------------------------------------ |
| Tipo de datos         | Estructurados, relaciones complejas | Flexibles, sin esquema fijo          |
| Velocidad             | Consultas complejas (JOINs)        | Lectura/escritura a milisegundos     |
| Escalado              | Vertical (más RAM/CPU)             | Automático e ilimitado               |
| Uso en ACME           | Pedidos, transacciones, usuarios   | Sesiones activas, logs, tiempo real  |

Ambas bases de datos coexisten porque cubren necesidades distintas: **RDS para datos relacionales del negocio** y **DynamoDB para datos de alta velocidad**.

## ¿Qué datos almacena VivaUsers para ACME?
```json
{
  "UserID": "usr-00123",
  "nombre": "Juan Pérez",
  "email": "juan@acme.com",
  "sesion_activa": true,
  "ultimo_acceso": "2026-04-05T10:30:00Z",
  "preferencias": { "tema": "oscuro", "idioma": "es" }
}
```
DynamoDB no requiere definir todos los campos de antemano: cada ítem puede tener atributos distintos.

## Crearemos lo siguiente:
| Recurso        | Detalle                                      |
| -------------- | -------------------------------------------- |
| Tabla DynamoDB | `VivaUsers` con clave primaria `UserID`      |
| Billing Mode   | PAY_PER_REQUEST (sin capacidad fija, pago por uso) |
| Tag proyecto   | Project: InfraestructuraViva                 |

## Ventaja del modo PAY_PER_REQUEST:
- No se paga capacidad reservada sin usar
- Escala automáticamente: 0 a millones de operaciones sin configuración
- Ideal para ACME en etapa inicial con tráfico variable

## Verificar
- Ir a **Amazon DynamoDB**
- Entrar a: **Tables**
- Deberías ver: **VivaUsers** con estado **Active** ✔
- Click en la tabla → pestaña **Items** → agregar un ítem de prueba:
  1. Click → **Create item**
  2. UserID: `usr-00001`
  3. Agregar atributos adicionales (nombre, email, etc.)
  4. Guardar y verificar que aparece en la tabla
