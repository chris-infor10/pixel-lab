## Nombre stack:
- infra-viva-monitoring

## ¿Qué es este componente?
Amazon CloudWatch es el servicio de monitoreo y observabilidad de AWS. Para **Soluciones Digitales ACME**, CloudWatch vigila el estado del servidor EC2 en tiempo real y genera alertas automáticas cuando el rendimiento se degrada, permitiendo respuesta proactiva antes de que los usuarios se vean afectados.

## Alarma creada:
| Parámetro           | Valor                             |
| ------------------- | --------------------------------- |
| Nombre              | CPUAlarmHigh                      |
| Métrica             | CPUUtilization (EC2)              |
| Namespace           | AWS/EC2                           |
| Estadística         | Average (promedio)                |
| Umbral              | Mayor a 70%                       |
| Período             | 300 segundos (5 minutos)          |
| Evaluaciones        | 2 períodos consecutivos           |
| Estado normal       | OK                                |
| Estado de alerta    | ALARM                             |

## ¿Por qué 2 evaluaciones consecutivas?
Esperar 2 períodos (10 minutos en total) evita **falsos positivos**: un pico de CPU momentáneo no dispara la alarma, solo una sobrecarga sostenida lo hace.

## Flujo de monitoreo para ACME:
```
VivaServer (EC2)
  │
  ▼  (cada 5 minutos)
CloudWatch recopila CPUUtilization
  │
  ├── CPU <= 70% por 2 períodos → Estado: OK ✔
  └── CPU >  70% por 2 períodos → Estado: ALARM ✘
                                      │
                                      ▼
                                 (alerta activa)
                              → revisar VivaServer
                              → identificar causa
                              → escalar instancia si es necesario
```

## Estados de la alarma:
| Estado             | Significado                                              |
| ------------------ | -------------------------------------------------------- |
| OK                 | CPU dentro del umbral, servidor funcionando bien ✔      |
| ALARM              | CPU superó 70% por 10 min → posible sobrecarga ✘        |
| INSUFFICIENT_DATA  | No hay datos suficientes (ej: instancia recién lanzada) |

## Prerequisito: obtener Instance ID
La alarma necesita el ID de la instancia EC2 a monitorear:

1. Ir a **Amazon EC2 → Instances**
2. Click en **VivaServer**
3. Copiar el **Instance ID** (ej: `i-01cb947fe9ecfbb47`)
4. Al lanzar el stack 06-monitoring, ingresar ese ID como parámetro

## Verificar
- Ir a **Amazon CloudWatch**
- Entrar a: **Alarms → All alarms**
- Deberías ver: **CPUAlarmHigh** con estado **OK** ✔
- Click en la alarma → pestaña **History**: ver el historial de cambios de estado

## Prueba de la alarma (para demostrar funcionamiento):
1. Conectarse por SSH a VivaServer
2. Ejecutar: `stress --cpu 4 --timeout 600` (requiere instalar `stress`)
3. Observar en CloudWatch cómo el estado pasa de OK → ALARM después de 10 minutos

## ¿Por qué es importante para ACME?
Sin monitoreo, un servidor sobrecargado solo se descubre cuando los usuarios reportan lentitud o errores. CloudWatch permite al equipo de ACME **detectar y reaccionar antes** de que el problema impacte la experiencia del usuario.
