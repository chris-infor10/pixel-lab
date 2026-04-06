## Nombre stack:
- micropay-ecr

## ¿Qué es ECR?
Amazon Elastic Container Registry (ECR) es un servicio de registro de contenedores Docker totalmente gestionado por AWS. Permite almacenar, administrar y desplegar imágenes Docker de forma segura.

## Repositorios creados:
| Repositorio | URI                                                         |
| ----------- | ----------------------------------------------------------- |
| usuarios    | 866240658957.dkr.ecr.us-east-1.amazonaws.com/usuarios      |
| pagos       | 866240658957.dkr.ecr.us-east-1.amazonaws.com/pagos         |

## Crearemos lo siguiente:
| Recurso            | Cantidad |
| ------------------ | -------- |
| Repositorio ECR    | 2        |
| Imagen usuarios    | 1        |
| Imagen pagos       | 1        |

## Pasos realizados:
1. Ir a ECR en la consola de AWS
2. Click → Create repository
3. Crear repositorio **usuarios**
4. Crear repositorio **pagos**

## Subir imágenes Docker (desde tu PC):

```bash
# Build y push usuarios
docker build -t usuarios .
docker tag usuarios:latest 866240658957.dkr.ecr.us-east-1.amazonaws.com/usuarios:latest
docker push 866240658957.dkr.ecr.us-east-1.amazonaws.com/usuarios:latest

# Build y push pagos
docker build -t pagos .
docker tag pagos:latest 866240658957.dkr.ecr.us-east-1.amazonaws.com/pagos:latest
docker push 866240658957.dkr.ecr.us-east-1.amazonaws.com/pagos:latest
```

## Verificar
- Ir a Amazon ECR
- Entrar a: Repositories
- Deberías ver: **usuarios** y **pagos**
- Cada uno con su imagen `latest` cargada
