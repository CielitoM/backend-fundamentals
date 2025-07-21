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


## 7. ¿Qué es un balanceador de carga y cómo ayuda a la escalabilidad de un sistema?

Un **balanceador de carga** (load balancer) es un componente que distribuye automáticamente el tráfico entrante entre múltiples servidores disponibles.

### ¿Por qué es importante?
- Mejora la **disponibilidad**: si un servidor falla, el tráfico se redirige a otro.
- Aumenta la **escalabilidad horizontal**: puedes agregar más servidores para manejar mayor carga.
- Optimiza el uso de recursos: evita que un servidor se sobrecargue mientras otros están ociosos.

### Tipos comunes:
- **Round-robin**: distribuye de forma circular.
- **Least connections**: al servidor con menos conexiones activas.
- **IP hash**: asigna según IP del cliente (útil para mantener sesión).

### Ilustración:

```plaintext
[Clientes] → [Balanceador de carga] → [Servidor A]
                                 ↘→ [Servidor B]
                                 ↘→ [Servidor C]
```
-falta agregar ilusitración-

Herramientas populares:

NGINX, HAProxy, AWS ELB, Traefik.

> El balanceo de carga es clave para crear aplicaciones tolerantes a fallos y escalables.


## 8. ¿Cuál es la diferencia entre escalabilidad horizontal y vertical?

La escalabilidad es la capacidad de un sistema para manejar un aumento en la carga de trabajo añadiendo recursos.

Existen dos tipos principales:

| Tipo de escalabilidad | Definición | Ejemplo | Ventajas | Desventajas |
|------------------------|------------|---------|----------|-------------|
| **Vertical (Scale Up)** | Añadir más recursos a una sola máquina (CPU, RAM) | Migrar a un servidor más potente | Simplicidad | Límite físico, puede ser costoso |
| **Horizontal (Scale Out)** | Añadir más máquinas al sistema | Añadir más instancias de servidores detrás de un balanceador de carga | Escalabilidad flexible y distribuida | Requiere coordinación y arquitectura distribuida |

### ¿Cuál es mejor?

Depende del caso:

- Para cargas pequeñas o simples, escalar verticalmente puede ser suficiente.
- Para sistemas grandes o con alta disponibilidad, escalar horizontalmente es más recomendable.

### Ejemplo práctico:

Supongamos que una API recibe cada vez más peticiones.

- **Escalado vertical:** Aumentar la RAM y CPU del servidor donde corre la API.
- **Escalado horizontal:** Desplegar 3 instancias de la API y usar un balanceador de carga (como Nginx o AWS ELB).

> En muchos sistemas reales, se usa una combinación de ambos enfoques.

## 9. ¿Qué es un balanceador de carga (load balancer) y cómo ayuda a escalar un sistema?

Un **balanceador de carga** es un componente que distribuye el tráfico entrante entre múltiples servidores o instancias, mejorando la disponibilidad y escalabilidad del sistema.

### Beneficios: 

| Beneficio | Descripción |
|-----------|-------------|
| **Alta disponibilidad** | Si un servidor falla, el tráfico se redirige a los otros |
| **Distribución de carga** | Evita que un solo servidor se sature |
| **Escalabilidad horizontal** | Permite añadir o quitar instancias según la demanda |
| **Mantenimiento sin downtime** | Se pueden actualizar servidores sin afectar al usuario final |

### Tipos de balanceadores:

| Tipo | Descripción |
|------|-------------|
| **Round Robin** | Asigna peticiones de forma rotativa |
| **Least Connections** | Asigna al servidor con menos conexiones activas |
| **IP Hash** | Asigna en base a la IP del cliente para mantener sesión |

### Ejemplo práctico:

Supongamos que una aplicación web tiene 3 instancias corriendo:

```text
Usuario → Load Balancer → [API #1, API #2, API #3]
```

Cuando un usuario hace una petición, el balanceador decide cuál instancia atenderá la solicitud. Esto permite escalar horizontalmente sin que el cliente lo note.

> Un balanceador de carga puede ser hardware (como F5) o software (como Nginx, HAProxy, o balanceadores en la nube como AWS ELB).

