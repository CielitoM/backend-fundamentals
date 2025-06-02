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