# Preguntas sobre Escalabilidad

## 1. ¿Qué es la escalabilidad horizontal y cómo se diferencia de la vertical?

- **Escalabilidad vertical:** Aumentar la capacidad de un solo servidor, por ejemplo, más CPU, memoria o disco.  
- **Escalabilidad horizontal:** Añadir más servidores al sistema para repartir la carga.

La escalabilidad horizontal es generalmente más flexible y económica para manejar grandes volúmenes de tráfico, ya que permite distribuir las solicitudes entre múltiples máquinas.

## 2. ¿Qué es la escalabilidad horizontal y cómo se implementa en un sistema backend?

La escalabilidad horizontal consiste en aumentar la capacidad del sistema agregando más máquinas o nodos, en lugar de mejorar el hardware de un solo servidor (escalabilidad vertical).

Implementación:
- Uso de balanceadores de carga para distribuir solicitudes.
- Replicación de bases de datos.
- Uso de microservicios para dividir funcionalidades.
- Caching distribuido para reducir carga en servidores.

## 3. ¿Qué es un balanceador de carga y cómo ayuda a la escalabilidad?

Un balanceador de carga distribuye automáticamente el tráfico entrante entre varios servidores para asegurar que ningún servidor se sobrecargue.

### Beneficios:
- Mejora la disponibilidad.
- Aumenta la tolerancia a fallos.
- Optimiza el uso de recursos.
- Facilita la escalabilidad horizontal.

### Tipos comunes:
- **Round-robin:** Distribución secuencial.
- **Least connections:** Al servidor con menos conexiones activas.
- **IP Hash:** Asocia clientes a servidores según su IP.

Usualmente se coloca al frente de múltiples instancias del backend.