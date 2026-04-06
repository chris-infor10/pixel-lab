## Nombre stack:
- micropay-monitoring

## ¿Qué es CloudWatch?
Amazon CloudWatch es el servicio de monitoreo y observabilidad de AWS. Permite recopilar logs, métricas y crear alarmas para los recursos en la nube.

## Log Groups creados:
| Log Group              | Servicio asociado       |
| ---------------------- | ----------------------- |
| /ecs/micropay/usuarios | Contenedor Usuarios ECS |
| /ecs/micropay/pagos    | Contenedor Pagos ECS    |

## Crearemos lo siguiente:
| Recurso         | Detalle                              |
| --------------- | ------------------------------------ |
| Log Group x2    | Uno por cada microservicio           |
| Log Retention   | 7 días                               |

## Pasos realizados para activar logs en ECS:
1. Ir a **ECS → Task Definitions**
2. Seleccionar `usuarios-task` → Crear nueva revisión (Create revision)
3. En la sección **Logging**, activar **Use log collection**
4. Click en **Create**
5. Repetir el mismo proceso para `pagos-task`

> ⚠️ Esto genera 2 revisiones por cada task (Revision 1 y Revision 2). La nueva revisión incluye la configuración de logs.

## Verificar
- Ir a **Amazon CloudWatch**
- Entrar a: **Logs → Log groups**
- Deberías ver:
  - `/ecs/micropay/usuarios`
  - `/ecs/micropay/pagos`
- Al hacer click en cada grupo verás los **log streams** con la ejecución de cada contenedor

## Qué puedes ver en los logs:
- Inicialización del servidor Node.js
- Peticiones HTTP recibidas (GET /usuarios, GET /pagos)
- Errores de conexión (si los hubiera)
- Tiempo de respuesta

## Prueba rápida (útil para el informe):
1. Llamar el endpoint en el navegador: `https://<api-url>/usuarios`
2. Ir a CloudWatch → Log groups → `/ecs/micropay/usuarios`
3. Abrir el log stream más reciente
4. Verificar que aparezca el registro de la petición recibida
