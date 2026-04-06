## Nombre stack:
- micropay-microservicios

## ¿Qué es CloudFormation?
AWS CloudFormation es el servicio de **Infraestructura como Código (IaC)** de AWS. Permite definir, provisionar y gestionar recursos de AWS mediante archivos de configuración YAML o JSON de forma automatizada y reproducible.

## Stack principal del proyecto:
El archivo `microservicios.yml` despliega **toda la infraestructura de MicroPay** en un único stack:

| Recurso                      | Nombre lógico               |
| ---------------------------- | --------------------------- |
| VPC                          | VPC                         |
| Internet Gateway             | InternetGateway             |
| VPC Gateway Attachment       | AttachGateway               |
| Public Subnet                | PublicSubnet                |
| Route Table                  | RouteTable                  |
| Route                        | Route                       |
| Subnet Route Table Assoc.    | SubnetRouteTableAssociation |
| Security Group               | SecurityGroup               |
| ECS Cluster                  | ECSCluster                  |
| Task Definition Usuarios     | TaskUsuarios                |
| Task Definition Pagos        | TaskPagos                   |
| ECS Service Usuarios         | ServiceUsuarios             |
| ECS Service Pagos            | ServicePagos                |

## Pasos realizados (click por click):
1. Ir a **CloudFormation** en la consola de AWS
2. Click → **Create stack**
3. Seleccionar → **Upload a template file**
4. Subir el archivo `microservicios.yml`
5. Click → **Next**
6. Ingresar nombre del stack: `micropay-microservicios`
7. Click → **Next → Next → Create stack**
8. Esperar que todos los recursos pasen a estado **CREATE_COMPLETE**

## Verificar
- Ir a **AWS CloudFormation**
- Entrar a: **Stacks**
- Deberías ver: `micropay-microservicios` con estado **CREATE_COMPLETE**
- En la pestaña **Resources** puedes ver todos los recursos creados
- En la pestaña **Events** puedes ver el historial de creación

## Ventajas de usar CloudFormation:
- Reproducibilidad: el mismo archivo genera la misma infraestructura
- Control de versiones: el YAML puede versionarse en Git
- Rollback automático: si falla la creación, AWS revierte los cambios
- Automatización: no requiere hacer click manual en cada servicio
