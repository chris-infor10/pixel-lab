## Nombre stack:
- micropay-apigateway

## ¿Qué es API Gateway?
Amazon API Gateway es un servicio totalmente gestionado que permite crear, publicar y gestionar APIs a escala. Actúa como "puerta de entrada" única a los microservicios.

## Rutas configuradas:
| Ruta       | Integración               | URL destino                    |
| ---------- | ------------------------- | ------------------------------ |
| /usuarios  | HTTP → IP pública Usuarios | http://\<IP_USUARIOS\>:3000   |
| /pagos     | HTTP → IP pública Pagos    | http://\<IP_PAGOS\>:3000      |

## URL pública de la API:
```
https://mpcvriaelk.execute-api.us-east-1.amazonaws.com/usuarios
https://mpcvriaelk.execute-api.us-east-1.amazonaws.com/pagos
```

## Crearemos lo siguiente:
| Recurso              | Detalle                                |
| -------------------- | -------------------------------------- |
| HTTP API             | 1 API Gateway tipo HTTP                |
| Integración Usuarios | HTTP hacia IP pública del contenedor   |
| Integración Pagos    | HTTP hacia IP pública del contenedor   |
| Ruta /usuarios       | GET → integración usuarios             |
| Ruta /pagos          | GET → integración pagos               |
| Stage                | $default (auto-deploy activado)        |

## Pasos realizados (click por click):
1. Ir a **API Gateway** en la consola AWS
2. Click → **Create API**
3. Seleccionar → **HTTP API**
4. Add integration → **HTTP**
5. Crear integración **usuarios**:
   - URL: `http://<IP_PUBLICA_USUARIOS>:3000`
6. Crear integración **pagos**:
   - URL: `http://<IP_PUBLICA_PAGOS>:3000`
7. Crear ruta `/usuarios` → integración usuarios
8. Crear ruta `/pagos` → integración pagos
9. En cada ruta: configurar **Authorization** (attach authorization)
10. Click → **Deploy**

## Verificar
- Ir a Amazon API Gateway
- Entrar a: APIs → tu HTTP API
- En Stages: copiar la **Invoke URL**
- Probar en navegador:
  - `https://<invoke-url>/usuarios`
  - `https://<invoke-url>/pagos`

## Errores comunes:
| Error          | Causa probable                        | Solución                          |
| -------------- | ------------------------------------- | --------------------------------- |
| Timeout        | Security Group no permite puerto 3000 | Revisar reglas de entrada del SG  |
| No conecta     | Contenedor no está corriendo          | Verificar estado del ECS Service  |
| No responde    | IP cambió (Fargate asigna IPs dinámicas) | Actualizar integración con nueva IP |

> 💡 En producción se recomienda usar un **Application Load Balancer** en lugar de IPs públicas directas.
