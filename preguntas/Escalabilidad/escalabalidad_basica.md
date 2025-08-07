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


## 10. ¿Cuál es la diferencia entre escalado horizontal y escalado vertical?


El **escalado horizontal** y el **escalado vertical** son dos estrategias para aumentar la capacidad de un sistema.

### Comparación:

| Característica            | Escalado Vertical                          | Escalado Horizontal                        |
|--------------------------|--------------------------------------------|--------------------------------------------|
| Definición              | Aumentar los recursos de una sola máquina | Añadir más máquinas al sistema             |
| Ejemplo                 | Más CPU/RAM en un servidor                | Más instancias de servidor trabajando en paralelo |
| Facilidad de implementación | Generalmente más fácil                   | Requiere arquitectura distribuida          |
| Límite físico           | Limitado por el hardware                  | Escalabilidad más flexible                 |
| Costos                 | Puede ser más costoso a gran escala       | Costos más predecibles y escalables        |

### Explicación:

- El **escalado vertical** es útil para sistemas monolíticos o cuando no se puede distribuir fácilmente la carga.
- El **escalado horizontal** es más común en sistemas modernos, especialmente con microservicios o arquitecturas cloud-native.

### Ejemplo:

Una base de datos puede escalar verticalmente aumentando la RAM. Sin embargo, si el sistema es una API con múltiples usuarios, lo ideal es escalar horizontalmente con más réplicas de la API detrás de un balanceador de carga.

> En sistemas grandes, se prefiere escalar horizontalmente para lograr alta disponibilidad y resiliencia.


## 11. ¿Qué es un balanceador de carga y cómo ayuda a escalar una aplicación?

Un **balanceador de carga** (load balancer) es un componente que distribuye el tráfico entrante entre múltiples servidores o instancias de una aplicación. Su objetivo es **optimizar el uso de recursos**, **evitar sobrecarga** en un solo servidor y **aumentar la disponibilidad** del sistema.

### ¿Cómo ayuda a escalar?

- **Escalabilidad horizontal**: Permite agregar más instancias de la aplicación fácilmente. El balanceador distribuye la carga entre ellas.
- **Alta disponibilidad**: Si una instancia falla, el balanceador puede redirigir las solicitudes a otras que estén funcionando.
- **Rendimiento**: Reduce los cuellos de botella al distribuir equitativamente las solicitudes.

### Tipos de balanceadores:

| Tipo                     | Descripción                                                                 |
|--------------------------|-----------------------------------------------------------------------------|
| Lógica de aplicación (L7) | Decide en función del contenido (por ejemplo, la URL o cabecera)              |
| Lógica de red (L4)         | Opera a nivel de puertos y protocolos como TCP o UDP                         |

### Ejemplo:

Supongamos que tenés una API con 3 instancias corriendo en contenedores. Un balanceador como NGINX o AWS ELB puede dirigir cada solicitud entrante a una de esas instancias según la carga actual, mejorando el rendimiento global.

> Es una pieza clave para escalar horizontalmente aplicaciones web y backend modernas.


## 12. ¿Cuál es la diferencia entre escalado horizontal y escalado vertical?

**Escalado horizontal** (scale out) y **escalado vertical** (scale up) son dos estrategias para aumentar la capacidad de una aplicación.

### Comparación:

| Característica            | Escalado Horizontal                              | Escalado Vertical                           |
|--------------------------|--------------------------------------------------|---------------------------------------------|
| Definición               | Añadir más máquinas/instancias                   | Añadir más recursos (CPU, RAM) a una máquina|
| Costo inicial            | Generalmente más costoso al inicio              | Puede ser más económico al principio        |
| Tolerancia a fallos      | Alta (varias instancias pueden fallar)          | Baja (un solo punto de fallo)               |
| Complejidad              | Mayor (requiere balanceadores, sincronización)  | Menor (menos infraestructura)               |
| Ejemplo                  | Añadir más servidores web detrás de un proxy     | Cambiar el servidor por uno más potente     |

### ¿Cuál conviene?

Depende del caso:

- **Sistemas distribuidos**: Prefieren escalado horizontal.
- **Aplicaciones monolíticas pequeñas**: Pueden escalar verticalmente al principio.

> Una arquitectura moderna suele comenzar con escalado vertical y, al crecer, adopta escalado horizontal para mejorar disponibilidad y rendimiento.


## 13. ¿Qué es un balanceador de carga y por qué es importante para la escalabilidad?

Un **balanceador de carga** es un componente que distribuye el tráfico de red o las solicitudes de los usuarios entre múltiples servidores. Su objetivo es garantizar que ningún servidor se sobrecargue y que los recursos se utilicen de forma eficiente.

### Importancia para la escalabilidad:

- **Permite el escalado horizontal**: Se pueden añadir más servidores detrás del balanceador sin cambiar la lógica de la aplicación.
- **Mejora la disponibilidad**: Si un servidor falla, el balanceador redirige el tráfico a los demás.
- **Optimiza el rendimiento**: Distribuye las solicitudes para evitar cuellos de botella.
- **Soporta sesiones persistentes** (sticky sessions), si es necesario.

### Tipos de balanceadores:

- **Nivel 4 (Transporte)**: Basado en IP y puertos (TCP/UDP).
- **Nivel 7 (Aplicación)**: Basado en contenido HTTP, cookies, etc.

- **Nginx**, **HAProxy**, **AWS Elastic Load Balancer (ELB)**, **Cloudflare**, etc.

> En arquitecturas distribuidas modernas, el balanceador de carga es un componente clave para garantizar escalabilidad, rendimiento y tolerancia a fallos.


## 14. ¿Cuál es la diferencia entre escalado vertical y escalado horizontal?

**Escalado vertical (scale-up)** consiste en aumentar la capacidad de un único servidor, por ejemplo:

- Agregar más CPU, memoria RAM o almacenamiento.
- Es más simple de implementar, pero tiene un límite físico y puede ser costoso.
- Ejemplo: cambiar una máquina con 4 GB de RAM por una de 16 GB.

**Escalado horizontal (scale-out)** consiste en agregar más servidores al sistema:

- Permite distribuir la carga entre múltiples instancias.
- Mejora la disponibilidad y la tolerancia a fallos.
- Es ideal para arquitecturas distribuidas como microservicios.
- Ejemplo: tener 3 servidores en lugar de uno solo.

### Consideraciones:

- El **escalado horizontal** es más complejo de implementar, requiere balanceadores de carga y sincronización de datos.
- Las **bases de datos relacionales** suelen escalar verticalmente, mientras que **NoSQL** se presta mejor para escalado horizontal.

>  En sistemas que requieren alta disponibilidad y capacidad de crecimiento, el escalado horizontal suele ser la mejor opción a largo plazo.

## 15. ¿Cómo medir si un sistema es escalable?

Un sistema es escalable si puede mantener o mejorar su rendimiento cuando aumenta la carga de trabajo, agregando recursos de manera eficiente.

### Para medir la escalabilidad, se pueden considerar varios indicadores:

- **Throughput (rendimiento):** número de peticiones procesadas por segundo.
- **Tiempo de respuesta (latencia):** cuánto tarda el sistema en responder a una petición.
- **Uso de recursos:** CPU, memoria, red y almacenamiento a medida que crece la carga.
- **Costo por rendimiento:** cuánto cuesta mantener cierto nivel de rendimiento al escalar.

### Ejemplo práctico:

Supongamos que un sistema maneja 1000 peticiones/segundo con 2 servidores. Al agregar 2 servidores más:

- Si ahora maneja 2000 peticiones/segundo ⇒ escalabilidad lineal (excelente).
- Si solo llega a 1300 ⇒ escalabilidad limitada.
- Si el rendimiento no mejora ⇒ posible cuello de botella o mal diseño.

### Conclusión:

La escalabilidad no solo se trata de agregar recursos, sino de hacerlo de forma **eficiente** y **proporcional** al crecimiento de la carga. Las pruebas de carga y estrés son esenciales para evaluarla correctamente.

## 16.  ¿Qué estrategias se pueden aplicar para manejar cargas pico en un sistema backend?

Para manejar cargas pico de forma eficiente, se pueden aplicar varias estrategias:

### 1. **Autoescalado (Autoscaling)**
- Configurar el sistema para que cree o elimine instancias automáticamente según el tráfico.
- Común en servicios como AWS EC2, Google Cloud Run o Kubernetes.

### 2. **Colas de trabajo (Work Queues)**
- Desacoplar tareas pesadas (como envío de correos o procesamiento de imágenes) del flujo principal.
- Herramientas comunes: RabbitMQ, Kafka, Celery.

### 3. **Caching**
- Usar sistemas de caché (como Redis o Memcached) para reducir la carga en la base de datos o servicios backend.
- Ideal para respuestas frecuentes o pesadas.

### 4. **CDN (Content Delivery Network)**
- Distribuir contenido estático (imágenes, scripts, estilos) a través de redes geográficamente distribuidas.
- Reduce la carga en servidores backend.

### 5. **Rate limiting y control de tráfico**
- Limitar la cantidad de peticiones por usuario para evitar abusos o DDoS.
- Puede implementarse en gateways, proxies o servicios API.

### Tip:
Planificar para cargas pico desde la fase de diseño mejora la disponibilidad y experiencia de usuario, especialmente en eventos importantes (lanzamientos, promociones, etc.).

### 17. ¿Cómo diseñarías un sistema escalable para procesar millones de solicitudes de imágenes por segundo, como lo haría un servicio de red social o CDN?

1. **Desacoplamiento mediante colas de mensajes**
   - Utilizar sistemas como **Kafka**, **RabbitMQ** o **SQS** para manejar las cargas asincrónicas.
   - Permite a los productores y consumidores escalar de forma independiente.

2. **Uso de una CDN (Content Delivery Network)**
   - Servir imágenes desde nodos cercanos al usuario para reducir latencia y carga de servidores principales.
   - Ejemplos: **Cloudflare**, **Akamai**, **Amazon CloudFront**.

3. **Balanceadores de carga**
   - Distribuir el tráfico entrante entre múltiples servidores de backend.
   - Escalado horizontal (agregar más instancias) para manejar más solicitudes.

4. **Procesamiento distribuido y microservicios**
   - Separar funciones en servicios individuales: redimensionamiento, compresión, filtros, etc.
   - Escalar cada uno por separado.

5. **Almacenamiento distribuido y caché**
   - Guardar imágenes en sistemas como **Amazon S3**, **Google Cloud Storage**, o soluciones on-premise como **Ceph**.
   - Cachear imágenes procesadas usando **Redis**, **Memcached** o CDN edge caches.

6. **Escalado automático**
   - Implementar autoescalado (horizontal) en función de métricas como uso de CPU, latencia, o tamaño de la cola de mensajes.
 
