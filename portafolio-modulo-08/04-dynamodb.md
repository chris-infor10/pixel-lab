## Nombre stack:
- serverless-dynamodb

## ¿Qué es DynamoDB?
Amazon DynamoDB es una base de datos NoSQL totalmente gestionada, serverless y de alto rendimiento. No requiere aprovisionar servidores ni gestionar esquemas complejos. Escala automáticamente según la demanda.

## Tabla creada:
| Propiedad       | Valor            |
| --------------- | ---------------- |
| Nombre          | Pedidos          |
| Clave primaria  | orderId (String) |
| Modo de pago    | PAY_PER_REQUEST  |
| Tipo            | NoSQL (clave-valor) |

## Crearemos lo siguiente:
| Recurso         | Detalle                          |
| --------------- | -------------------------------- |
| Tabla DynamoDB  | 1 tabla llamada Pedidos          |
| Partition Key   | orderId (tipo String)            |
| Billing Mode    | PAY_PER_REQUEST (sin capacidad fija) |

## Flujo de datos:
```
API Gateway
  │
  ▼
Lambda PedidosFunction
  │  genera: orderId = uuid aleatorio
  │  inserta: { orderId, detalle }
  ▼
DynamoDB tabla: Pedidos
```

## Ejemplo de ítem insertado:
```json
{
  "orderId": "a3f1c8e2-9b34-4e7d-bb12-1234abcd5678",
  "detalle": "Pedido creado desde Lambda"
}
```

## Pasos realizados para verificar:
1. Ir a **DynamoDB** en la consola AWS
2. Click en **Tables → Pedidos**
3. Ir a la pestaña **Explore items**
4. Deberías ver los registros insertados por Lambda cada vez que se llama `/pedidos`

## Prueba rápida (útil para el informe):
1. Llamar el endpoint en el navegador: `https://<api-url>/pedidos`
2. Ir a DynamoDB → Tabla Pedidos → Explore items
3. Verificar que aparece un nuevo ítem con un `orderId` único

## Verificar
- Ir a **Amazon DynamoDB**
- Entrar a: **Tables**
- Deberías ver: **Pedidos** con estado **Active**
- En **Explore items**: ver los registros insertados por la función Lambda
