## Nombre stack:
- serverless-inteligente

## ¿Qué es CloudFormation?
AWS CloudFormation es el servicio de **Infraestructura como Código (IaC)** de AWS. Permite definir y provisionar todos los recursos de AWS en un solo archivo YAML, de forma automatizada, reproducible y sin errores manuales.

## Stack principal del proyecto:
El archivo `serverless.yml` despliega **toda la infraestructura del proyecto Serverless Inteligente** en un único stack:

| Recurso                  | Nombre lógico             | Servicio    |
| ------------------------ | ------------------------- | ----------- |
| Bucket S3                | S3Bucket                  | S3          |
| Tabla DynamoDB           | PedidosTable              | DynamoDB    |
| Función Lambda Usuarios  | LambdaUsuarios            | Lambda      |
| Función Lambda Pedidos   | LambdaPedidos             | Lambda      |
| HTTP API                 | HttpApi                   | API Gateway |
| Integración Usuarios     | UsuariosIntegration       | API Gateway |
| Integración Pedidos      | PedidosIntegration        | API Gateway |
| Ruta GET /usuarios       | UsuariosRoute             | API Gateway |
| Ruta GET /pedidos        | PedidosRoute              | API Gateway |
| Stage $default           | Stage                     | API Gateway |
| Permiso Lambda Usuarios  | LambdaPermissionUsuarios  | Lambda      |
| Permiso Lambda Pedidos   | LambdaPermissionPedidos   | Lambda      |

## Output del stack:
| Output  | Valor                                                          |
| ------- | -------------------------------------------------------------- |
| ApiURL  | https://\<id\>.execute-api.us-east-1.amazonaws.com            |

## Pasos realizados (click por click):
1. Ir a **CloudFormation** en la consola de AWS
2. Click → **Create stack**
3. Seleccionar → **Upload a template file**
4. Subir el archivo `serverless.yml`
5. Click → **Next**
6. Ingresar nombre del stack: `serverless-inteligente`
7. Click → **Next → Next → Create stack**
8. Esperar que todos los recursos pasen a estado **CREATE_COMPLETE**

## Verificar
- Ir a **AWS CloudFormation**
- Entrar a: **Stacks**
- Deberías ver: `serverless-inteligente` con estado **CREATE_COMPLETE**
- En la pestaña **Resources**: todos los recursos creados listados
- En la pestaña **Outputs**: la URL del API Gateway lista para usar
- En la pestaña **Events**: historial completo de creación de cada recurso

## Comparación con proyecto 07 (MicroPay):
| Aspecto                | Proyecto 07 (MicroPay)   | Proyecto 08 (Serverless) |
| ---------------------- | ------------------------ | ------------------------ |
| Compute                | ECS Fargate (contenedor) | Lambda (serverless)      |
| Base de datos          | No incluida              | DynamoDB                 |
| Sitio web              | No incluido              | S3 static hosting        |
| Gestión de servidores  | AWS gestiona Fargate     | Sin servidores           |
| Escalado               | DesiredCount manual      | Automático               |
| Costo                  | Por hora de contenedor   | Por invocación           |
