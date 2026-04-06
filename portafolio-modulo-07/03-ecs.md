## Nombre stack:
- micropay-ecs

## ¿Qué es ECS Fargate?
Amazon Elastic Container Service (ECS) con Fargate es un motor de orquestación de contenedores serverless. No requiere administrar servidores — AWS gestiona la infraestructura subyacente.

## Componentes creados:
```
ECS Cluster: MicroPayCluster
│
├── Task Definition: usuarios-task
│    └── Container: usuarios (puerto 3000)
│         └── Image: 866240658957.dkr.ecr.us-east-1.amazonaws.com/usuarios:latest
│
└── Task Definition: pagos-task
     └── Container: pagos (puerto 3000)
          └── Image: 866240658957.dkr.ecr.us-east-1.amazonaws.com/pagos:latest
```

## Crearemos lo siguiente:
| Recurso               | Detalle                          |
| --------------------- | -------------------------------- |
| ECS Cluster           | 1 cluster compartido             |
| Task Definition       | 2 (una por microservicio)        |
| ECS Service Usuarios  | DesiredCount: 1, Fargate         |
| ECS Service Pagos     | DesiredCount: 1, Fargate         |
| CPU por tarea         | 256 (.25 vCPU)                   |
| Memoria por tarea     | 512 MB                           |

## Pasos realizados:
1. Ir a ECS en la consola de AWS
2. Click → Clusters → verificar que el cluster esté **ACTIVE**
3. En Services: ambos servicios deben estar en estado **RUNNING**
4. En Tasks: cada task debe tener una **IP pública asignada**

## Obtener IP pública de las tareas (para API Gateway):
1. ECS → Cluster → Tasks
2. Click en una task (usuarios o pagos)
3. Copiar el campo **Public IP**

## Verificar logs (CloudWatch):
Para activar logs en ECS:
1. Ir a Task Definitions
2. Crear nueva revisión del task (ej: usuarios-task)
3. En la sección **Logging**, activar **Use log collection**
4. Crear → repetir para pagos

> ⚠️ Nota: Fargate asigna IPs dinámicas. Si el servicio se reinicia, la IP cambia. En producción se recomienda usar un **Application Load Balancer**.
