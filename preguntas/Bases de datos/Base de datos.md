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
```

## 4. ¿Qué es la normalización y por qué es importante?

La normalización es el proceso de organizar los datos en una base de datos para reducir la redundancia y mejorar la integridad de los datos.

### Beneficios:
- Elimina datos duplicados.
- Mejora la consistencia.
- Facilita el mantenimiento.
- Mejora la eficiencia de almacenamiento.

### Formas normales comunes:
1. **Primera forma normal (1FN):** Elimina grupos repetitivos.
2. **Segunda forma normal (2FN):** Elimina dependencias parciales.
3. **Tercera forma normal (3FN):** Elimina dependencias transitivas.

## 5. ¿Qué es un índice en una base de datos y cómo mejora el rendimiento?

Un índice es una estructura auxiliar que permite realizar búsquedas más rápidas sobre una o varias columnas de una tabla.

### Ventajas:
- Acelera búsquedas (`SELECT`).
- Mejora consultas con `WHERE`, `JOIN`, `ORDER BY`.

### Desventajas:
- Ocupa espacio adicional.
- Ralentiza inserciones, actualizaciones y eliminaciones (`INSERT`, `UPDATE`, `DELETE`) porque el índice también debe actualizarse.

### Ejemplo SQL:
```sql
CREATE INDEX idx_usuario_email ON usuarios(email);
```
Este índice mejora las búsquedas que filtran por email.

## 6. ¿Cuál es la diferencia entre normalización y desnormalización?

- **Normalización:**  
  - Proceso de dividir una base de datos en tablas pequeñas para eliminar redundancias.
  - Mejora la consistencia y el mantenimiento.
  - Ej: separar `usuario` y `dirección` en dos tablas relacionadas.

- **Desnormalización:**  
  - Se agregan duplicados deliberadamente para mejorar rendimiento.
  - Útil en lecturas frecuentes o sistemas analíticos.
  - Ej: incluir la dirección directamente en la tabla de `pedidos`.

### Comparación rápida:

| Criterio        | Normalización       | Desnormalización     |
|-----------------|---------------------|-----------------------|
| Redundancia     | Mínima              | Aumentada             |
| Consistencia    | Alta                | Potencialmente menor  |
| Rendimiento     | Lecturas más lentas | Lecturas más rápidas  |

## 7. ¿Cuál es la diferencia entre una transacción y un bloqueo (lock) en una base de datos?

- **Transacción:** Unidad lógica de trabajo que debe ser completa o no hacerse (ACID).
- **Bloqueo:** Mecanismo para evitar que múltiples procesos accedan al mismo recurso de forma conflictiva.

Una transacción puede implicar varios bloqueos internos. Por ejemplo, al actualizar filas, se bloquean temporalmente para evitar condiciones de carrera.

### Ejemplo:

```sql
BEGIN;
UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;
COMMIT;
```