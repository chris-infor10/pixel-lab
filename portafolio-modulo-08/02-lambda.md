## Nombre stack:
- serverless-lambda

## ¿Qué es AWS Lambda?
AWS Lambda es un servicio de computación **serverless** que ejecuta código en respuesta a eventos sin necesidad de aprovisionar ni administrar servidores. Se paga únicamente por el tiempo de ejecución del código.

## Funciones creadas:
| Función           | Runtime    | Descripción                                 |
| ----------------- | ---------- | ------------------------------------------- |
| UsuariosFunction  | Python 3.9 | Retorna mensaje de confirmación del servicio |
| PedidosFunction   | Python 3.9 | Inserta un pedido nuevo en DynamoDB          |

## Crearemos lo siguiente:
| Recurso              | Cantidad |
| -------------------- | -------- |
| Lambda Function      | 2        |
| IAM Role (LabRole)   | 1 (reutilizado) |

## Código de cada función:

### UsuariosFunction
```python
def handler(event, context):
    return {
        'statusCode': 200,
        'body': 'Microservicio Usuarios funcionando'
    }
```

### PedidosFunction
```python
import json
import boto3
import uuid

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('Pedidos')

def handler(event, context):
    order_id = str(uuid.uuid4())

    table.put_item(
        Item={
            'orderId': order_id,
            'detalle': 'Pedido creado desde Lambda'
        }
    )

    return {
        'statusCode': 200,
        'body': json.dumps({
            'mensaje': 'Pedido creado',
            'orderId': order_id
        })
    }
```

## Pasos realizados para probar (click por click):

### Probar UsuariosFunction
1. Ir a **Lambda** en la consola AWS
2. Click en `UsuariosFunction`
3. Ir a la pestaña **Test**
4. Crear un evento de prueba con payload vacío: `{}`
5. Click → **Test**
6. Resultado esperado: `"Microservicio Usuarios funcionando"`

### Probar PedidosFunction
1. Ir a **Lambda** en la consola AWS
2. Click en `PedidosFunction`
3. Ir a la pestaña **Test**
4. Crear un evento de prueba con payload vacío: `{}`
5. Click → **Test**
6. Resultado esperado: JSON con `"mensaje": "Pedido creado"` y un `orderId` generado

## Verificar
- Ir a **AWS Lambda**
- Entrar a: **Functions**
- Deberías ver: `UsuariosFunction` y `PedidosFunction` con estado **Active**
- En cada función, pestaña **Monitor**: ver métricas de invocaciones y duración
