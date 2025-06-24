# Preguntas sobre Arquitectura

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