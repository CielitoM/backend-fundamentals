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

## 4. ¿Por qué se utiliza un sistema de caché en una arquitectura backend?

El caching permite guardar respuestas o resultados temporales de operaciones costosas (como consultas a bases de datos) para acelerar el acceso posterior.

### Beneficios:
- Reduce la carga sobre la base de datos.
- Mejora el tiempo de respuesta.
- Disminuye el costo computacional.

### Tipos de caché:
- **En memoria:** Redis, Memcached.
- **HTTP Cache:** Varnish, headers como `Cache-Control`.
- **Local en cliente o servidor.**

### Ejemplo:
Una API que devuelve productos populares puede usar caché para no hacer la misma consulta cada segundo.

>  “Cachear lo que cambia poco, calcular lo que cambia mucho”.

## 5. ¿Qué problemas pueden surgir al manejar múltiples peticiones concurrentes y cómo se mitigan?

Problemas comunes:

- **Condiciones de carrera:** dos procesos acceden al mismo recurso a la vez.
- **Deadlocks:** dos procesos esperan recursos que el otro tiene.
- **Contención:** acceso simultáneo a un recurso limita el rendimiento.

### Estrategias para mitigarlos:

- Uso de **bloqueos (locks)** y **transacciones** en bases de datos.
- Uso de **colas de tareas** (ej: RabbitMQ) para procesar solicitudes en orden.
- Implementar **rate limiting** y **reintentos con backoff**.
- Diseñar servicios **idempotentes** y **stateless**.

### Ejemplo:
Una API bancaria debe evitar que dos transferencias simultáneas vacíen la misma cuenta sin control.

## 6. ¿Cómo se puede escalar un servicio stateless usando caching?

Un servicio stateless no mantiene información entre peticiones, lo que facilita su escalabilidad horizontal. Para mejorar rendimiento aún más, se puede usar caché.

### Ejemplo:
- El backend es stateless (no guarda sesión).
- Se agrega una capa de cache (como Redis) para almacenar resultados de consultas frecuentes.

### Ventajas:
- El backend puede replicarse en múltiples servidores.
- La caché reduce carga en base de datos y mejora velocidad.
- Ideal para sistemas distribuidos y APIs públicas.

### Ilustración:

```plaintext
[Cliente] → [Balanceador] → [Instancia A] ↘
                              [Redis Cache] → [Base de Datos]
                           ↗ [Instancia B]
```
-falta agregar ilusitración-