# Preguntas sobre Bases de Datos

## 1. ¿Qué es una transacción en bases de datos y qué significa que sea ACID?

Una transacción es una secuencia de operaciones en una base de datos que se ejecutan como una unidad indivisible, asegurando que se completen todas o ninguna.

ACID es un acrónimo que describe las propiedades que garantizan la integridad de una transacción:

- **Atomicidad:** Todo o nada.  
- **Consistencia:** La base de datos pasa de un estado válido a otro.  
- **Aislamiento:** Las transacciones concurrentes no interfieren entre sí.  
- **Durabilidad:** Los cambios se mantienen aunque haya fallos después de la confirmación.

## 2. ¿Qué es una base de datos relacional y qué ventajas tiene?

Una base de datos relacional organiza los datos en tablas con filas y columnas, y usa claves primarias y foráneas para establecer relaciones entre tablas.

Ventajas:
- Integridad y consistencia de datos.
- Uso de SQL para consultas complejas.
- Soporte para transacciones ACID.
- Facilidad para modelar datos estructurados.

## 3. ¿Cuál es la diferencia entre una consulta `INNER JOIN` y una `LEFT JOIN`?

- `INNER JOIN`: Devuelve solo las filas que tienen coincidencias en ambas tablas.
- `LEFT JOIN`: Devuelve todas las filas de la tabla de la izquierda, y si no hay coincidencia en la tabla derecha, completa con `NULL`.

### Ejemplo:

```sql
SELECT usuarios.nombre, pedidos.id
FROM usuarios
INNER JOIN pedidos ON usuarios.id = pedidos.usuario_id;