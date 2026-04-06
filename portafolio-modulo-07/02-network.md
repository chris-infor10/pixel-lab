## Nombre stack:
- micropay-network

## Información redes
- PublicSubnet → 10.0.1.0/24

## Infraestructura actual es básicamente:
```
VPC 10.0.0.0/16
│
└── PublicSubnet (10.0.1.0/24)  us-east-1
     └── Permite IP pública automática (MapPublicIpOnLaunch: true)
```

## Crearemos lo siguiente:
| Recurso                      | Cantidad |
| ---------------------------- | -------- |
| VPC                          | 1        |
| Public Subnet                | 1        |
| Internet Gateway             | 1        |
| Route Table                  | 1        |
| Subnet Route Table Association | 1      |

## Security Group (puerto 3000):
| Regla    | Protocolo | Puerto | Origen    |
| -------- | --------- | ------ | --------- |
| Inbound  | TCP       | 3000   | 0.0.0.0/0 |

El puerto 3000 es el que exponen ambos microservicios (Usuarios y Pagos).

## Verificar
- Ir a Amazon VPC
- Entrar a: Your VPCs
- Deberías ver la VPC con CIDR: **10.0.0.0/16**
- En Subnets: ver **PublicSubnet** con CIDR 10.0.1.0/24
- En Security Groups: ver el grupo con regla de entrada en puerto 3000
