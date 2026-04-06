## Nombre stack:
- cloudsecure-cloudwatch

## ¿Qué es este componente?
Implementación de **monitoreo activo** con Amazon CloudWatch. Se configura una alarma que detecta comportamientos anómalos en el uso de CPU de instancias EC2, generando una alerta automática cuando el umbral definido es superado.

## Alarma creada:
| Parámetro           | Valor                         |
| ------------------- | ----------------------------- |
| Nombre              | CPUAlarm                      |
| Métrica             | CPUUtilization (EC2)          |
| Namespace           | AWS/EC2                       |
| Umbral              | Mayor a 70%                   |
| Periodo             | 300 segundos (5 minutos)      |
| Periodos evaluados  | 1                             |
| Estado normal       | OK                            |
| Estado de alerta    | ALARM                         |

## Crearemos lo siguiente:
| Recurso           | Detalle                                     |
| ----------------- | ------------------------------------------- |
| CloudWatch Alarm  | Alarma de CPU > 70% en instancias EC2       |

## Flujo de la alarma:
```
EC2 Instance
  │
  ▼  (cada 5 minutos)
CloudWatch recopila CPUUtilization
  │
  ├── CPU <= 70%  →  Estado: OK  ✔
  └── CPU >  70%  →  Estado: ALARM ✘
                        │
                        ▼
                    (alerta detectada)
```

## Estados de la alarma:
| Estado       | Significado                                              |
| ------------ | -------------------------------------------------------- |
| OK           | La métrica está dentro del umbral definido ✔            |
| ALARM        | La métrica superó el umbral → comportamiento anómalo ✘  |
| INSUFFICIENT_DATA | No hay suficientes datos para evaluar aún          |

## Pasos realizados (click por click):

### Verificar alarma creada
1. Ir a **CloudWatch** en la consola AWS
2. Click en **Alarms → All alarms**
3. Deberías ver: `CPUAlarm`
4. Estado inicial: **OK** (o INSUFFICIENT_DATA si no hay EC2 activa)

### Ver el historial de la alarma
1. Click en la alarma `CPUAlarm`
2. Pestaña **History**: ver todos los cambios de estado registrados
3. Pestaña **Details**: ver la configuración de la métrica y umbral

### Simular una alerta (para el informe):
En caso de querer demostrar el funcionamiento:
1. Lanzar una instancia EC2
2. Ejecutar un proceso intensivo de CPU (ej: stress test)
3. Observar cómo el estado cambia de OK → ALARM en CloudWatch

## Verificar
- Ir a **Amazon CloudWatch**
- Entrar a: **Alarms → All alarms**
- Deberías ver: `CPUAlarm` con estado **OK** ✔
- Threshold configurado: **> 70% CPU** ✔

## ¿Por qué es importante para Blue Wave?
El monitoreo activo permite detectar comportamientos anómalos antes de que afecten el servicio. Un uso elevado de CPU puede indicar un ataque de denegación de servicio (DDoS), un proceso malicioso o una sobrecarga inesperada, permitiendo respuesta inmediata ante incidentes de seguridad.
