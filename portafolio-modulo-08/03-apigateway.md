## Nombre stack:
- serverless-apigateway

## ¿Qué es API Gateway?
Amazon API Gateway es un servicio totalmente gestionado que permite crear y publicar APIs a escala. En este proyecto actúa como punto de entrada único hacia las funciones Lambda, permitiendo invocarlas vía HTTP desde el navegador o herramientas como Postman.

## Flujo de la arquitectura:
```
Usuario
  │
  ▼
API Gateway (HTTP API)
  ├── GET /usuarios  →  Lambda UsuariosFunction
  └── GET /pedidos   →  Lambda PedidosFunction
```

## Rutas configuradas:
| Ruta       | Método | Integración             | Tipo      |
| ---------- | ------ | ----------------------- | --------- |
| /usuarios  | GET    | Lambda UsuariosFunction | AWS_PROXY |
| /pedidos   | GET    | Lambda PedidosFunction  | AWS_PROXY |

## URL pública de la API:
```
https://13b3fozwl7.execute-api.us-east-1.amazonaws.com/usuarios
https://13b3fozwl7.execute-api.us-east-1.amazonaws.com/pedidos
```

## Crearemos lo siguiente:
| Recurso              | Detalle                                |
| -------------------- | -------------------------------------- |
| HTTP API             | 1 API Gateway tipo HTTP                |
| Integración Usuarios | AWS_PROXY hacia LambdaUsuarios         |
| Integración Pedidos  | AWS_PROXY hacia LambdaPedidos          |
| Ruta GET /usuarios   | Conecta con integración usuarios       |
| Ruta GET /pedidos    | Conecta con integración pedidos        |
| Stage $default       | Auto-deploy activado                   |
| Lambda Permission x2 | Permite que API Gateway invoque Lambda |

## Diferencia con el proyecto 07 (MicroPay):
| Proyecto 07 (MicroPay)        | Proyecto 08 (Serverless)              |
| ----------------------------- | ------------------------------------- |
| HTTP_PROXY a IP pública ECS   | AWS_PROXY a función Lambda directa    |
| Contenedor corriendo en Fargate | Sin servidor (serverless)            |
| IP dinámica (puede cambiar)   | ARN fijo de Lambda (estable)          |

## Verificar
- Ir a **API Gateway** en la consola AWS
- Entrar a: **APIs → ServerlessAPI**
- En **Routes**: verificar `/usuarios` y `/pedidos`
- En **Integrations**: verificar que apuntan a las funciones Lambda correctas
- Probar en el navegador:
  - `https://<invoke-url>/usuarios` → retorna: `Microservicio Usuarios funcionando`
  - `https://<invoke-url>/pedidos` → retorna JSON con `orderId` generado
