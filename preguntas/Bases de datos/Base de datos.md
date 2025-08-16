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
Esta operación se hace de forma atómica, y se aplican bloqueos hasta el commit.

## 8. ¿Qué ventajas y desventajas tiene usar un ORM en lugar de SQL directo?

**ORM (Object-Relational Mapping)** permite manipular bases de datos usando objetos en el lenguaje de programación.

### Ventajas:
- Abstracción del acceso a datos.
- Menor código repetitivo (boilerplate).
- Facilita mantenimiento y portabilidad.

### Desventajas:
- Menor control sobre consultas complejas.
- Puede generar consultas ineficientes si no se revisa.
- Curva de aprendizaje en algunos casos.

**SQL directo** es más flexible y controlado, pero más detallado y manual.

> En proyectos grandes, se suelen usar ambos enfoques según el caso.


## 9. ¿Qué es el problema N+1 y cómo se soluciona?

El **problema N+1** ocurre cuando, al obtener una lista de entidades, se realiza una consulta adicional por cada una para acceder a sus datos relacionados.

### Ejemplo típico (ORM mal usado):

```python
# Se obtienen todos los usuarios
usuarios = Usuario.query.all()

# Por cada usuario, se consulta su perfil (1 consulta adicional por usuario)
for usuario in usuarios:
    print(usuario.perfil.nombre)
```
Si hay 100 usuarios → 1 consulta para usuarios + 100 para perfiles → 101 consultas.

### Solución:

Usar carga anticipada (eager loading) o JOIN para traer los datos relacionados de una vez.

Ejemplo en SQLAlchemy:

```SQLAlchemy
usuarios = Usuario.query.options(joinedload(Usuario.perfil)).all()
```
Esto hace una sola consulta con JOIN y evita el problema.


## 10. ¿Cuáles son los principales tipos de JOIN en SQL y en qué casos se usan?

Un **JOIN** se usa para combinar filas de dos o más tablas basándose en una condición relacionada entre ellas (generalmente claves foráneas).

### Tipos principales:

| Tipo de JOIN   | Descripción                                               | Ejemplo común                      |
|----------------|-----------------------------------------------------------|------------------------------------|
| **INNER JOIN** | Devuelve solo las filas que coinciden en ambas tablas     | Usuarios con pedidos               |
| **LEFT JOIN**  | Devuelve todas las filas de la tabla izquierda y sus coincidencias (si existen) en la derecha | Todos los usuarios y sus pedidos (aunque no tengan) |
| **RIGHT JOIN** | Similar a LEFT JOIN pero al revés                         | Pedidos y sus usuarios (aunque no registrados) |
| **FULL JOIN**  | Devuelve todas las filas que tengan coincidencia en una u otra tabla | Unión completa de ambas tablas     |

### Ejemplo con LEFT JOIN:
```sql
SELECT u.nombre, p.id_pedido
FROM usuarios u
LEFT JOIN pedidos p ON u.id = p.id_usuario;
```

Esto muestra a todos los usuarios, tengan o no pedidos.

Elegir el JOIN adecuado evita errores lógicos  y mejora el rendimiento de la consulta.

## 11. ¿Qué es un índice en una base de datos y cuáles son sus ventajas y desventajas?

Un **índice** es una estructura de datos adicional que permite acelerar la búsqueda de registros en una tabla, de forma similar a un índice en un libro.

### Ventajas:
- **Acelera las consultas** `SELECT`, especialmente en columnas con filtros (`WHERE`, `JOIN`, `ORDER BY`, etc.).
- Mejora el rendimiento de búsquedas y relaciones entre tablas.

### Desventajas:
- **Ocupa espacio adicional** en disco.
- **Ralentiza las operaciones de escritura** (`INSERT`, `UPDATE`, `DELETE`), ya que el índice también debe actualizarse.

### Tipos comunes de índices:
- **Índice único:** no permite valores repetidos.
- **Índice compuesto:** usa varias columnas.
- **Índice de texto completo:** útil para búsqueda de palabras o frases.

### Ejemplo SQL:
```sql
CREATE INDEX idx_nombre ON usuarios(nombre);
```

### Buenas prácticas:

Indexar columnas usadas frecuentemente en filtros o joins.

No indexar todo: muchos índices pueden perjudicar el rendimiento general.

> El uso de índices debe balancear velocidad de lectura y costo de mantenimiento en escritura.

## 12. ¿Qué es la normalización en bases de datos y por qué es importante?

La **normalización** es el proceso de organizar las columnas y tablas de una base de datos para **reducir la redundancia de datos y mejorar la integridad**.

### Objetivos:
- Evitar duplicación de datos.
- Asegurar dependencias lógicas entre datos.
- Facilitar el mantenimiento y la consistencia.

### Formas normales más comunes:

| Forma Normal | Requisitos                                                    |
|--------------|---------------------------------------------------------------|
| 1NF          | Elimina grupos repetitivos. Cada celda debe tener un solo valor. |
| 2NF          | Cumple 1NF y elimina dependencias parciales (para claves compuestas). |
| 3NF          | Cumple 2NF y elimina dependencias transitivas. |

### Ejemplo:

Una tabla no normalizada:

| cliente | dirección       | producto   |
|---------|------------------|------------|
| Ana     | Calle 1          | Zapatos    |
| Ana     | Calle 1          | Camisa     |

→ Está duplicando la dirección.

**Normalizado:**

- `clientes(id, nombre, direccion)`
- `ventas(id_cliente, producto)`

### ¿Cuándo desnormalizar?
- En sistemas de lectura intensiva, para rendimiento (OLAP).
- Cuando se prioriza la velocidad sobre la integridad.

> La normalización mejora la calidad del diseño y facilita evitar errores difíciles de detectar en producción.

## 13. ¿Qué es una transacción en una base de datos y qué significan las propiedades ACID?

Una **transacción** es una unidad lógica de trabajo que agrupa una o más operaciones sobre la base de datos. Se asegura que todas esas operaciones se ejecuten completamente o ninguna, manteniendo la coherencia.

### Propiedades ACID:

| Propiedad   | Significado                                                                 |
|-------------|------------------------------------------------------------------------------|
| **A**tomicidad  | Todas las operaciones ocurren completamente o ninguna lo hace.            |
| **C**onsistencia | La base de datos pasa de un estado válido a otro estado válido.          |
| **I**solamiento  | Las transacciones concurrentes no se interfieren entre sí.               |
| **D**urabilidad  | Una vez realizada una transacción, los cambios persisten incluso ante fallos. |

### Ejemplo:

```sql
BEGIN;

UPDATE cuentas SET saldo = saldo - 100 WHERE id = 1;
UPDATE cuentas SET saldo = saldo + 100 WHERE id = 2;

COMMIT;
```

Si alguna de las actualizaciones falla, se puede hacer ROLLBACK y los datos no quedan en un estado inconsistente.

### ¿Por qué es importante?

Evita errores como transferencias incompletas.

Garantiza integridad en aplicaciones bancarias, financieras, etc.

Imprescindible en sistemas concurrentes o distribuidos.


> Las propiedades ACID son la base de la confiabilidad en los sistemas que usan bases de datos relacionales.

## 14
