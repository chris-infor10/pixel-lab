## Nombre stack:
- infra-viva-network

## ¿Qué es este componente?
La red es la base de toda la infraestructura de **Soluciones Digitales ACME**. Amazon VPC (Virtual Private Cloud) permite aislar todos los recursos dentro de un entorno seguro y privado en AWS, simulando una red corporativa tradicional pero con la flexibilidad de la nube.

## Arquitectura de red implementada:
```
VPC: 10.0.0.0/16  (VivaVPC)
│
├── PublicSubnetA   (10.0.1.0/24)  us-east-1a  ← EC2, recursos con acceso a internet
├── PublicSubnetB   (10.0.2.0/24)  us-east-1b  ← Redundancia multi-AZ
│
├── PrivateSubnetA  (10.0.3.0/24)  us-east-1a  ← RDS, bases de datos
└── PrivateSubnetB  (10.0.4.0/24)  us-east-1b  ← RDS, bases de datos (backup)
```

## ¿Por qué subredes públicas Y privadas?
| Tipo de subred | Acceso desde internet | Uso en ACME                        |
| -------------- | --------------------- | ---------------------------------- |
| Pública        | Sí (vía IGW)          | Servidor EC2, aplicación web       |
| Privada        | No (aislada)          | Base de datos RDS (datos sensibles) |

Separar recursos públicos de privados es una **práctica de seguridad fundamental**: la base de datos nunca queda expuesta directamente a internet.

## Crearemos lo siguiente:
| Recurso                       | Cantidad | Detalle                                  |
| ----------------------------- | -------- | ---------------------------------------- |
| VPC                           | 1        | CIDR 10.0.0.0/16, DNS habilitado         |
| Public Subnets                | 2        | us-east-1a y us-east-1b                  |
| Private Subnets               | 2        | us-east-1a y us-east-1b                  |
| Internet Gateway              | 1        | Permite salida a internet desde la VPC   |
| Route Table (pública)         | 1        | Dirige tráfico público al IGW            |
| Subnet Route Table Assoc. x2  | 2        | Asocia subnets públicas a la route table |

## Arquitectura multi-AZ (alta disponibilidad):
Al distribuir los recursos en dos zonas de disponibilidad (us-east-1a y us-east-1b), si una zona falla, la otra continúa operando. Esto es equivalente a tener servidores en dos edificios distintos.

## Pasos para verificar:
1. Ir a **Amazon VPC** en la consola AWS
2. Entrar a: **Your VPCs** → ver **VivaVPC** con CIDR 10.0.0.0/16
3. Entrar a: **Subnets** → ver las 4 subredes creadas
4. Entrar a: **Internet Gateways** → ver el gateway adjunto a VivaVPC
5. Entrar a: **Route Tables** → ver la ruta 0.0.0.0/0 apuntando al IGW

## Orden de despliegue:
> ⚠️ Este stack debe desplegarse **primero** antes que cualquier otro, ya que todos los demás recursos (EC2, RDS) dependen de esta red.
