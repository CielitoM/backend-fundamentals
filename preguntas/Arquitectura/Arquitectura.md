# 20 Preguntas sobre Arquitectura 

## 1. ¿Qué es una arquitectura basada en microservicios y qué ventajas ofrece?

Una arquitectura de microservicios divide una aplicación en pequeños servicios independientes, cada uno con una única responsabilidad. Estos servicios se comunican mediante APIs (por ejemplo, REST o gRPC).

### Ventajas:

- **Escalabilidad independiente:** Se pueden escalar solo los servicios que lo necesiten.
- **Despliegue autónomo:** Cada servicio puede actualizarse sin afectar al resto.
- **Aislamiento de errores:** Fallos en un servicio no colapsan todo el sistema.
- **Facilita el trabajo en equipo:** Los equipos pueden trabajar en diferentes servicios de forma paralela.

### Desventajas a tener en cuenta:

- Mayor complejidad de infraestructura.
- Comunicación entre servicios puede volverse más costosa y requiere diseño cuidadoso.

## 2. ¿Cuáles son las diferencias entre una arquitectura monolítica y una de microservicios?

### Monolítica:
- Toda la aplicación es una sola unidad.
- Simpler to deploy.
- Más difícil de escalar por partes.

### Microservicios:
- Divide la aplicación en componentes independientes.
- Cada componente tiene su propia lógica y base de datos.
- Escalabilidad, mantenimiento y despliegue por separado.

### Comparación:

| Característica        | Monolítica               | Microservicios            |
|-----------------------|--------------------------|----------------------------|
| Escalabilidad         | Global                   | Por servicio               |
| Complejidad inicial   | Baja                     | Alta                       |
| Mantenimiento         | Difícil a gran escala    | Más fácil                  |
| Aislamiento de fallos | Bajo                     | Alto                       |

## 3. ¿Qué significa que una API siga el estilo RESTful?

Una API es RESTful si sigue los principios del estilo arquitectónico REST, como:

- Uso de métodos HTTP correctos (GET, POST, PUT, DELETE).
- URLs que representan recursos, no acciones (ej: `/usuarios/1`).
- Stateless: cada request contiene toda la información necesaria.
- Soporte para formatos estándar como JSON.
- Uso de códigos de estado HTTP apropiados.

### Ejemplo de buenas prácticas REST:

- `GET /productos` → obtener todos los productos  
- `POST /productos` → crear un nuevo producto  
- `PUT /productos/5` → actualizar producto con ID 5  
- `DELETE /productos/5` → eliminar producto con ID 5

## 4. ¿Qué significa que un sistema sea stateless y por qué es útil?

- **Stateless:** Cada petición contiene toda la información necesaria. El servidor no recuerda peticiones anteriores.
- **Stateful:** El servidor mantiene información de estado entre peticiones (como sesiones).

### Ventajas del diseño stateless:

- Escalabilidad: se pueden balancear peticiones entre múltiples servidores fácilmente.
- Simplicidad: menos dependencia del servidor.
- Tolerancia a fallos: más fácil de reiniciar sin perder contexto.

### Ejemplo:
Una API REST bien diseñada es stateless: cada `request` lleva el `token`, los datos y no requiere historial previo.

## 5. ¿Cuáles son las capas típicas de una arquitectura backend bien estructurada?

Una arquitectura backend suele dividirse en capas para mejorar la organización y la mantenibilidad.

### Capas típicas:

1. **Capa de presentación** (opcional en backend puro): API, controllers.
2. **Capa de negocio (servicio):** lógica principal, validaciones.
3. **Capa de acceso a datos (DAO o repositorio):** interacción con la base de datos.
4. **Capa de persistencia:** base de datos o sistema de archivos.

Este diseño promueve separación de responsabilidades y testeo modular.

### Ejemplo de estructura:

```bash
/backend
├── controllers/
├── services/
├── repositories/
└── models/
```

## 6. ¿Qué es el patrón CQRS y cuándo conviene usarlo?

**CQRS (Command Query Responsibility Segregation)** es un patrón arquitectónico que **separa las operaciones de lectura (Query)** de las **operaciones de escritura (Command)**.

### ¿Cómo funciona?

- Se usan **modelos distintos** para leer y para escribir datos.
- Esto permite optimizar cada parte por separado.

### Ejemplo:

- **Command:** `POST /ordenes` → escribe una nueva orden.
- **Query:** `GET /ordenes/por-cliente/5` → lee las órdenes del cliente 5.

### Beneficios:
- Escalabilidad independiente para lectura y escritura.
- Menor acoplamiento.
- Permite adaptar diferentes bases de datos o tecnologías por tipo de operación.

### ¿Cuándo usarlo?

- En sistemas con alto tráfico y diferencias marcadas entre consultas y escrituras.
- En aplicaciones que usan event sourcing o microservicios.

> CQRS no es necesario en todos los proyectos, pero es muy útil en sistemas grandes y críticos.



## 7. ¿Cuál es la diferencia entre una arquitectura monolítica y una basada en microservicios?

| Característica       | Monolito                          | Microservicios                        |
|----------------------|-----------------------------------|----------------------------------------|
| Estructura           | Una sola aplicación unificada     | Múltiples servicios independientes     |
| Despliegue           | Se despliega todo junto           | Se despliega cada servicio por separado |
| Escalabilidad        | Escalabilidad global              | Escalabilidad por componente           |
| Fallos               | Un fallo puede afectar todo       | Fallos aislados                        |
| Comunicación         | Interna (funciones, módulos)      | API (HTTP, mensajería, etc.)          |
| Complejidad inicial  | Menor                             | Mayor (requiere más infraestructura)   |

### Ventajas del monolito:
- Simplicidad inicial
- Más fácil de desarrollar y depurar al principio

### Ventajas de microservicios:
- Flexibilidad tecnológica (cada servicio puede tener su stack)
- Escalabilidad independiente
- Mejor aislamiento de fallos

### ¿Cuándo usar cada uno?
- Monolito: proyectos pequeños o etapas tempranas.
- Microservicios: sistemas grandes, equipos distribuidos, necesidades de alta escalabilidad.

> Una buena práctica es **comenzar con un monolito bien estructurado** que permita migrar a microservicios si el proyecto crece.


## 8. ¿Qué es una arquitectura orientada a eventos y cuándo se recomienda usarla?

Una **arquitectura orientada a eventos (event-driven architecture)** es un enfoque donde los componentes del sistema se comunican y reaccionan a eventos.

### Conceptos clave:

- **Evento**: una acción o cambio de estado ("pedido creado", "usuario registrado").
- **Emisor (publisher)**: componente que genera el evento.
- **Suscriptor (subscriber)**: componente que reacciona al evento.
- **Bus de eventos o cola**: canal que transporta los eventos (ej: Kafka, RabbitMQ).

### Ventajas:
- **Desacoplamiento**: los componentes no necesitan conocerse entre sí.
- **Escalabilidad**: cada servicio puede escalar independientemente.
- **Asincronía**: ideal para procesos que no necesitan respuesta inmediata.

### Ejemplo:
1. Un usuario hace una compra.
2. Se emite el evento `CompraRealizada`.
3. Otros servicios lo procesan:
   - Notificaciones envía un email.
   - Inventario descuenta productos.
   - Facturación genera la factura.

```plaintext
[Compra] → (Evento) → [Cola de eventos]
                          ↙        ↘
                [Notificaciones] [Inventario]
                          ↘        ↘
                      [Facturación] ...
```

### ¿Cuándo usarla?

En sistemas distribuidos o microservicios.

Cuando hay tareas desacopladas, asincrónicas o extensibles.

Cuando se necesita alta disponibilidad y flexibilidad.


> Pensar en eventos permite construir sistemas más modulares, adaptables y preparados para crecer.


## 9. ¿Qué es Domain-Driven Design (DDD) y por qué es importante en arquitectura de software?

**Domain-Driven Design (DDD)** es un enfoque de diseño de software centrado en el **modelo del dominio** y en la **colaboración cercana con expertos del negocio** para construir software alineado con la realidad del problema.

### Principios clave:

- **Modelo de dominio:** representación precisa del negocio dentro del código.
- **Lenguaje ubicuo (Ubiquitous Language):** vocabulario compartido entre desarrolladores y expertos del dominio.
- **Separación de capas:** presentación, aplicación, dominio, infraestructura.
- **Contexto delimitado (Bounded Context):** una parte clara y autónoma del sistema donde un modelo es válido.

### Beneficios:
- El software refleja fielmente la lógica del negocio.
- Mejora la comunicación entre equipos técnicos y no técnicos.
- Favorece el diseño modular, especialmente útil en microservicios.

### Ejemplo de capas:
```plaintext
[Presentación] → [Aplicación] → [Dominio] → [Infraestructura]
```

¿Cuándo aplicar DDD?

En sistemas complejos con mucha lógica de negocio.

Cuando el entendimiento del dominio es crítico para el éxito del proyecto.


> DDD no es solo técnica: es colaboración constante con el negocio para construir software realmente útil y sostenible.

## 10. ¿Qué es el patrón MVC y cómo se aplica en el desarrollo backend?

**MVC (Model-View-Controller)** es un patrón de arquitectura que separa una aplicación en tres componentes principales:

| Componente | Responsabilidad                                 |
|------------|--------------------------------------------------|
| **Model**  | La lógica del negocio y acceso a datos          |
| **View**   | La presentación o salida (en backend, a veces es JSON) |
| **Controller** | Intermediario que recibe solicitudes, ejecuta lógica y entrega respuestas |

### Aplicación en backend:

En un backend web típico, por ejemplo usando Django, Laravel o Express:

- El **Controlador** gestiona las rutas y recibe las solicitudes HTTP.
- El **Modelo** se conecta a la base de datos.
- La **Vista** puede ser una plantilla HTML o una respuesta JSON.

### Ejemplo (pseudocódigo):

```python
# Controller
@app.route("/usuarios/<id>")
def obtener_usuario(id):
    usuario = UsuarioModel.obtener(id)  # Modelo
    return jsonify(usuario.to_dict())   # Vista (JSON)
```

### Ventajas:

Separación clara de responsabilidades.

Facilita el mantenimiento, testeo y escalabilidad.

Reutilización del modelo en diferentes vistas (web, móvil, etc.)


> MVC es uno de los patrones más usados en backend por su simplicidad y organización clara.


## 11. ¿Qué es la Clean Architecture y qué ventajas ofrece en el desarrollo de aplicaciones backend?

**Clean Architecture** es un enfoque de diseño de software propuesto por Robert C. Martin (Uncle Bob) que busca lograr sistemas **independientes del framework, la base de datos o la interfaz de usuario**, promoviendo la separación de responsabilidades en capas bien definidas.

### Capas principales (de adentro hacia afuera):

1. **Entidad (Entities):** lógica de negocio y reglas empresariales.
2. **Casos de uso (Use Cases):** lógica específica de aplicación.
3. **Interfaces (Interface Adapters):** controladores, gateways, serializadores.
4. **Infraestructura (Frameworks & Drivers):** base de datos, HTTP, etc.

```plaintext
[Framework] → [Interfaces] → [Casos de Uso] → [Entidades]
```

### Ventajas:

Bajo acoplamiento y alta cohesión.

Facilidad de testeo (capas internas pueden probarse sin base de datos).

Sustituibilidad: puedes cambiar el motor de base de datos o framework sin reescribir la lógica de negocio.

Longevidad del código: las reglas del negocio no dependen de tecnologías externas.


### Ejemplo:

En una API, el controlador HTTP no contiene lógica de negocio. Solo delega a un caso de uso, que luego usa un repositorio para obtener los datos.

> Clean Architecture permite construir sistemas mantenibles, testeables y resistentes al paso del tiempo.

## 12. ¿Qué es la Arquitectura Hexagonal y cuál es su propósito?

La **Arquitectura Hexagonal** (también llamada **Ports and Adapters**) es un enfoque que busca aislar la lógica del dominio central de las dependencias externas, como bases de datos, servicios web, interfaces gráficas, etc.

### Componentes clave:

- **Dominio central:** contiene la lógica de negocio pura.
- **Puertos (Ports):** interfaces que definen cómo interactuar con el dominio.
- **Adaptadores (Adapters):** implementaciones concretas que conectan los puertos con tecnologías externas (HTTP, DB, etc.).

```plaintext
            ┌───────────────────────────────┐
            │        Interfaces             │
            │  (HTTP, DB, CLI, etc.)        │
            └──────┬────────────┬───────────┘
                   │            │
             ┌─────▼────┐  ┌────▼────┐
             │ Adapter  │  │ Adapter │
             └─────┬────┘  └────┬────┘
                   ▼            ▼
                ┌───────────────────┐
                │      Port         │
                └────────┬──────────┘
                         ▼
                ┌───────────────────┐
                │   Application     │
                └───────────────────┘
```

### Ventajas:

Aislamiento de lógica de negocio.

Alta testabilidad (el núcleo no depende de infraestructura).

Flexibilidad: puedes cambiar adaptadores sin tocar el núcleo.


### Ejemplo:

Puedes cambiar PostgreSQL por MongoDB sin alterar tu lógica de negocio, solo reemplazando el adaptador correspondiente.

> Esta arquitectura promueve sistemas desacoplados, resistentes al cambio y fácilmente extensibles.

## 13. ¿Qué significa que un servicio sea "sin estado" (stateless) y por qué es importante en arquitectura backend?

Un **servicio sin estado** es aquel que **no guarda información de la sesión del usuario ni de peticiones anteriores** entre una solicitud y otra.

Cada solicitud se maneja de forma **independiente**, como si fuera la primera.

### Ventajas:
- **Escalabilidad sencilla**: cualquier instancia puede manejar cualquier solicitud.
- **Tolerancia a fallos**: se puede reiniciar una instancia sin perder contexto.
- **Facilidad para balanceo de carga**: no se necesita "recordar" a qué servidor fue un usuario.

### Ejemplo:

- En una API RESTful, cada solicitud incluye toda la información necesaria (por ejemplo, el token JWT del usuario), y el servidor no guarda nada entre peticiones.

```http
GET /perfil
Authorization: Bearer <token>
```
El servidor valida el token, procesa y responde. No necesita recordar qué pasó antes.

Contraste con servicios con estado:

En servicios con estado, el servidor mantiene variables de sesión, lo que complica la escalabilidad y la recuperación ante fallos.


> Diseñar servicios sin estado es una de las claves para construir sistemas distribuidos modernos y escalables.


### 14. Explica las diferencias entre una arquitectura monolítica y una arquitectura de microservicios, y menciona un caso de uso en el que elegirías cada una.

1. **Definición:**
   - **Monolítica:** Toda la aplicación se construye como una sola unidad.  
   - **Microservicios:** La aplicación se divide en servicios pequeños e independientes que se comunican entre sí.

2. **Ventajas y desventajas:**
   - **Monolítica:**
     - Ventajas: Simplicidad inicial, fácil de desplegar.
     - Desventajas: Difícil de escalar por módulos, cambios afectan a todo el sistema.
   - **Microservicios:**
     - Ventajas: Escalabilidad independiente por servicio, facilidad para usar tecnologías distintas.
     - Desventajas: Complejidad en despliegue, monitoreo y comunicación.

3. **Casos de uso:**
   - **Monolítica:** Aplicaciones pequeñas o MVPs donde la velocidad de desarrollo inicial es prioritaria.
   - **Microservicios:** Sistemas grandes y distribuidos que requieren escalabilidad y despliegues independientes.

### 16.¿Cuáles son los principios de diseño de una arquitectura orientada a microservicios y cómo ayudan a escalar un sistema?

Los principios clave de microservicios son:

1. **Servicios independientes**:
   - Cada microservicio implementa una funcionalidad específica y puede desarrollarse, desplegarse y escalarse de manera independiente.

2. **Comunicación ligera**:
   - Los servicios se comunican mediante protocolos ligeros como HTTP/REST, gRPC o mensajería asíncrona.

3. **Descentralización de datos**:
   - Cada microservicio puede tener su propia base de datos o almacenamiento, evitando un punto único de fallo y permitiendo escalar por servicio.

4. **Tolerancia a fallos**:
   - Implementar mecanismos de resiliencia como *circuit breakers*, retries y *timeouts* para que un fallo en un servicio no colapse todo el sistema.

5. **Automatización y despliegue independiente**:
   - Cada microservicio puede actualizarse y desplegarse sin afectar al resto del sistema.

**Beneficios para la escalabilidad:**
- Permite **escalar solo los servicios que requieren más recursos**.
- Facilita la adopción de diferentes tecnologías según el caso de uso.
- Mejora la disponibilidad y la tolerancia a fallos del sistema completo.

### 17. ¿Qué es la arquitectura de microservicios y en qué se diferencia de la arquitectura monolítica?

- **Arquitectura monolítica:**
  - Toda la aplicación se construye como un solo bloque indivisible.
  - Los módulos comparten la misma base de datos y entorno de ejecución.
  - **Ventajas:** simplicidad inicial, fácil de desplegar.
  - **Desventajas:** difícil de escalar por partes, cambios pequeños pueden requerir desplegar toda la aplicación.

- **Arquitectura de microservicios:**
  - La aplicación se divide en servicios independientes que se comunican entre sí mediante APIs (REST, gRPC, mensajería, etc.).
  - Cada microservicio puede tener su propia base de datos y ciclo de vida.
  - **Ventajas:** escalabilidad granular, despliegues independientes, mayor resiliencia.
  - **Desventajas:** mayor complejidad en la comunicación, monitoreo y orquestación.

### 17. ¿Qué es una arquitectura orientada a eventos (Event-Driven Architecture) y cuándo es recomendable usarla?

- **Definición:**
  - Es un estilo arquitectónico en el que los componentes se comunican entre sí mediante la producción, detección y reacción a eventos.
  - Los eventos son notificaciones que indican que algo sucedió en el sistema (por ejemplo, "pedido creado", "usuario registrado").

- **Características:**
  - Usa colas de mensajes o buses de eventos (ej. Kafka, RabbitMQ).
  - Favorece la **desacoplación** entre productores y consumidores.
  - Los servicios pueden reaccionar de manera asíncrona a los cambios.

- **Ventajas:**
  - Escalabilidad y flexibilidad al manejar grandes volúmenes de eventos.
  - Alta resiliencia: los fallos en un consumidor no bloquean el sistema completo.
  - Ideal para sistemas distribuidos y en tiempo real.

- **Cuándo usarla:**
  - En aplicaciones que requieren procesar flujos de datos en tiempo real.
  - Cuando se busca un sistema altamente escalable y desacoplado.
  - En arquitecturas de microservicios que necesitan comunicación asíncrona.

### 18. ¿Cuáles son las principales diferencias entre una arquitectura monolítica y una basada en microservicios?  

### Respuesta:
- **Arquitectura monolítica:**
  - Toda la aplicación está empaquetada y desplegada como una única unidad.
  - Las funciones (autenticación, pagos, reportes, etc.) están altamente acopladas.
  - Escalar implica replicar toda la aplicación, incluso las partes que no lo necesitan.
  - Más sencilla de desarrollar y desplegar al inicio, pero difícil de mantener en sistemas grandes.

- **Arquitectura de microservicios:**
  - La aplicación se divide en servicios pequeños, independientes y especializados.
  - Cada servicio se comunica generalmente mediante **