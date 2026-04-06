## Nombre stack:
- infra-viva-messaging

## ¿Qué es este componente?
El sistema de mensajería integra **Amazon SNS** (Simple Notification Service) y **Amazon SQS** (Simple Queue Service) para desacoplar los componentes de la arquitectura de **Soluciones Digitales ACME**. Esto permite que la aplicación publique eventos sin preocuparse de quién los procesa, mejorando la escalabilidad y resiliencia del sistema.

## Recursos creados:
| Recurso       | Nombre              | Servicio | Rol                                    |
| ------------- | ------------------- | -------- | -------------------------------------- |
| Topic SNS     | viva-notifications  | SNS      | Publica eventos del sistema            |
| Cola SQS      | viva-queue          | SQS      | Recibe y almacena mensajes para procesar |
| Queue Policy  | VivaQueuePolicy     | SQS      | Autoriza a SNS a escribir en la cola   |
| Subscription  | VivaSubscription    | SNS→SQS  | Conecta el Topic con la Cola           |

## Flujo de mensajería para ACME:
```
Aplicación (EC2 VivaServer)
    │
    │ publica evento: "Pedido creado"
    ▼
Topic SNS: viva-notifications
    │
    │ distribuye a todos los suscriptores
    ▼
Cola SQS: viva-queue
    │
    │ almacena el mensaje hasta que sea procesado
    ▼
Procesamiento (worker, Lambda, u otro servicio)
```

## ¿Por qué desacoplar con SNS + SQS?
| Sin mensajería (acoplado)               | Con SNS + SQS (desacoplado)                   |
| --------------------------------------- | --------------------------------------------- |
| Si el procesador falla, se pierde el evento | El mensaje espera en la cola hasta ser procesado |
| El servidor debe esperar la respuesta   | El servidor publica y continúa sin esperar    |
| Difícil de escalar                      | Se pueden agregar más workers en paralelo     |
| Un fallo afecta toda la cadena          | Los componentes fallan de forma independiente |

## Ejemplo de evento real para ACME:
```
Escenario: usuario realiza un pedido

1. VivaServer publica en SNS:
   { "evento": "pedido_creado", "pedidoId": "PED-1234", "usuario": "usr-00123" }

2. SNS entrega el mensaje a viva-queue (SQS)

3. El procesador lee la cola y ejecuta:
   - Enviar email de confirmación al usuario
   - Actualizar inventario en RDS
   - Registrar la transacción en DynamoDB
```

## Crearemos lo siguiente:
| Recurso          | Detalle                                          |
| ---------------- | ------------------------------------------------ |
| SNS Topic        | `viva-notifications` para publicar eventos       |
| SQS Queue        | `viva-queue` para almacenar mensajes             |
| Queue Policy     | Permite a SNS enviar mensajes a SQS              |
| SNS Subscription | Protocolo SQS: conecta el topic a la cola        |

## Prueba rápida paso a paso:

### 1. Publicar un mensaje desde SNS:
1. Ir a **Amazon SNS → Topics**
2. Click en `viva-notifications`
3. Click → **Publish message**
4. Message body: `Pedido creado`
5. Click → **Publish**

### 2. Verificar que llegó a SQS:
1. Ir a **Amazon SQS → Queues**
2. Click en `viva-queue`
3. Click → **Send and receive messages**
4. Bajar hasta **Receive messages** → Click → **Poll for messages**
5. Verás el mensaje `Pedido creado` → click en él para ver el detalle

Esto demuestra la **comunicación entre servicios** de la arquitectura ACME.

## Verificar recursos creados
- Ir a **Amazon SNS → Topics** → ver `viva-notifications` ✔
- Ir a **Amazon SQS → Queues** → ver `viva-queue` ✔
- Click en `viva-queue` → pestaña **Subscriptions** → ver la suscripción activa con SNS ✔
