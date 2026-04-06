## Nombre stack:
- serverless-monitoring

## ¿Qué es CloudWatch?
Amazon CloudWatch es el servicio de monitoreo y observabilidad de AWS. En proyectos serverless, CloudWatch captura automáticamente los logs de cada invocación de Lambda, permitiendo ver el comportamiento, duración y errores de las funciones.

## Log Groups creados automáticamente por Lambda:
| Log Group                        | Función asociada    |
| -------------------------------- | ------------------- |
| /aws/lambda/UsuariosFunction     | Lambda Usuarios     |
| /aws/lambda/PedidosFunction      | Lambda Pedidos      |

> ⚡ A diferencia del proyecto 07 (ECS), en Lambda los logs se crean automáticamente en CloudWatch sin configuración adicional.

## Crearemos lo siguiente:
| Recurso         | Detalle                                  |
| --------------- | ---------------------------------------- |
| Log Group x2    | Uno por cada función Lambda              |
| Log Retention   | 7 días                                   |

## Qué puedes ver en los logs de cada función:

### UsuariosFunction:
- Inicio de invocación (`START RequestId`)
- Respuesta retornada
- Fin de invocación (`END RequestId`) con duración y memoria usada

### PedidosFunction:
- Inicio de invocación
- Conexión a DynamoDB
- `orderId` generado
- Item insertado correctamente
- Fin de invocación con duración

## Pasos para ver los logs (click por click):
1. Ir a **CloudWatch** en la consola AWS
2. Entrar a: **Logs → Log groups**
3. Buscar `/aws/lambda/UsuariosFunction`
4. Click en el log group → ver **Log streams**
5. Click en el stream más reciente → ver los registros de ejecución
6. Repetir con `/aws/lambda/PedidosFunction`

## Prueba rápida (útil para el informe):
1. Llamar los endpoints: `/usuarios` y `/pedidos` desde el navegador
2. Ir a CloudWatch → Log groups → cada función
3. Verificar que aparecen nuevos log streams con la ejecución

## Verificar
- Ir a **Amazon CloudWatch**
- Entrar a: **Logs → Log groups**
- Deberías ver:
  - `/aws/lambda/UsuariosFunction`
  - `/aws/lambda/PedidosFunction`
- En cada log stream: invocaciones, mensajes de ejecución y errores (si existen)
