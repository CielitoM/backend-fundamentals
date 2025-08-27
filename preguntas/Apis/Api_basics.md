# Preguntas básicas sobre APIs

## 1. ¿Qué es una API REST y cuáles son sus principales características?

Una API REST (Representational State Transfer) es un estilo arquitectónico para diseñar servicios web que permite la comunicación entre sistemas mediante protocolos HTTP.

Las principales características son:

- **Stateless:** Cada petición es independiente y no guarda estado en el servidor.  
- **Recursos:** Los datos se representan como recursos identificados por URLs.  
- **Métodos HTTP:** Uso de métodos estándar como GET (obtener), POST (crear), PUT (actualizar), DELETE (eliminar).  
- **Formato:** Generalmente se usa JSON para intercambiar datos.  
- **Escalabilidad:** Fácil de escalar por su diseño simple y separación clara de cliente y servidor.
![alt text](image.png)

## 2. ¿Qué diferencias hay entre REST y GraphQL?

- **REST:** Exposición de múltiples endpoints por recurso. Las respuestas suelen ser fijas.
- **GraphQL:** Un solo endpoint que permite al cliente especificar qué datos necesita.

### Comparación:

| Característica        | REST                          | GraphQL                            |
|-----------------------|-------------------------------|-------------------------------------|
| Número de endpoints   | Varios                        | Uno solo                            |
| Flexibilidad de datos | Limitada                      | Alta (cliente define la respuesta)  |
| Overfetching          | Sí                            | No                                  |
| Underfetching         | Sí                            | No                                  |
| Curva de aprendizaje  | Baja                          | Media/Alta                          |

### Ejemplo de consulta GraphQL:

```graphql
query {
  usuario(id: 1) {
    nombre
    email
  }
}
```

## 3. ¿Qué técnicas puedes usar para mejorar el rendimiento de una API?

- **Caching:** Evita consultas repetidas accediendo a datos ya procesados.
- **Paginación:** Controla la cantidad de datos enviados en cada respuesta.
- **Compresión (gzip):** Reduce el tamaño del cuerpo de la respuesta.
- **Índices en la base de datos:** Mejoran la velocidad de búsqueda.
- **Balanceo de carga:** Distribuye el tráfico entre varios servidores.
- **Throttling y rate limiting:** Controlan el uso excesivo de la API.

Estas prácticas mejoran la eficiencia, reducen la latencia y escalan mejor bajo alta demanda.

## 4. ¿Cuáles son las diferencias clave entre REST y SOAP?

| Característica     | REST                              | SOAP                            |
|--------------------|-----------------------------------|----------------------------------|
| Estilo             | Arquitectura                      | Protocolo                        |
| Formato de datos   | JSON (generalmente)               | XML solamente                    |
| Simplicidad        | Simple, ligero                    | Complejo, más pesado             |
| Transporte         | HTTP                              | HTTP, SMTP, más                  |
| Velocidad          | Alta (más eficiente)              | Menor (más verboso)              |
| Estándares         | No estrictos                      | Estrictos (WS-* standards)       |
| Uso común          | APIs web modernas                 | Integraciones empresariales      |

REST es preferido actualmente por ser más flexible y eficiente, especialmente en aplicaciones web modernas.

## 5. ¿Qué significa que una operación HTTP sea idempotente?

Una operación es **idempotente** si aplicarla una o múltiples veces produce el mismo resultado final.

### Métodos HTTP idempotentes:

- `GET`: no modifica nada.
- `PUT`: reemplaza un recurso; aplicar varias veces no cambia el efecto.
- `DELETE`: eliminar el mismo recurso varias veces sigue dejándolo eliminado.

`POST` **no es idempotente**, porque cada llamada puede crear un recurso nuevo.

### Ejemplo:
```http
PUT /usuarios/123/email
Body: { "email": "nuevo@mail.com" }
```
No importa si se envía una vez o cinco, el email termina siendo el mismo.

## 6. ¿Cuáles son los principales códigos de estado HTTP que deberías manejar en una API?

| Código | Significado                  | Uso común                             |
|--------|------------------------------|----------------------------------------|
| 200    | OK                           | Respuesta exitosa                      |
| 201    | Created                      | Recurso creado exitosamente            |
| 204    | No Content                   | Respuesta sin contenido (ej: DELETE)   |
| 400    | Bad Request                  | Error en los datos enviados            |
| 401    | Unauthorized                 | Falta autenticación                    |
| 403    | Forbidden                    | Usuario autenticado pero sin permisos |
| 404    | Not Found                    | Recurso no existe                      |
| 500    | Internal Server Error        | Error inesperado del servidor          |

Usar correctamente los códigos mejora la comprensión del cliente sobre el estado de su solicitud.

## 7. ¿Cómo debe manejar los errores una API correctamente?
Una buena API debe:

1. **Usar códigos de estado HTTP adecuados:**
   - 400: solicitud mal formada
   - 401: no autenticado
   - 403: sin permisos
   - 404: recurso no encontrado
   - 500: error interno

2. **Retornar mensajes claros en formato consistente (por ejemplo, JSON):**

```json
{
  "error": "Usuario no encontrado",
  "codigo": 404
}
```
3. **Evitar exponer información sensible (pila de errores, rutas internas, etc.).**

4. **Registrar errores internamente para diagnóstico sin exponerlos al cliente.**

Ejemplo (Express.js):
```javascript
Copiar código
app.get("/usuarios/:id", async (req, res) => {
  try {
    const user = await buscarUsuario(req.params.id);
    if (!user) return res.status(404).json({ error: "Usuario no encontrado" });
    res.json(user);
  } catch (err) {
    console.error(err);
    res.status(500).json({ error: "Error interno del servidor" });
  }
});
```
Nota:
El manejo adecuado de errores mejora la experiencia del cliente y facilita el mantenimiento.


## 8. ¿Por qué es importante versionar una API y cuáles son las formas comunes de hacerlo?

El **versionado** permite hacer cambios en una API (como modificar o eliminar campos) sin afectar a los clientes que dependen de versiones anteriores.

### ¿Por qué es importante?
- Permite mantener compatibilidad con clientes antiguos.
- Facilita introducir mejoras o cambios estructurales progresivamente.
- Reduce el riesgo de romper integraciones en producción.

### Formas comunes de versionar una API:

| Estrategia         | Ejemplo                       | Comentario                         |
|--------------------|-------------------------------|------------------------------------|
| En la URL          | `/api/v1/usuarios`            | Muy usada, clara y visible         |
| En el header       | `Accept: application/vnd.miapi.v1+json` | Más limpio, pero menos visible     |
| En el parámetro    | `/usuarios?version=1`         | Simple, pero menos recomendable    |

### Buenas prácticas:
- Documentar claramente los cambios entre versiones.
- Mantener versiones antiguas mientras haya clientes activos.
- Evitar romper cambios en versiones activas sin previo aviso.


## 9. ¿Qué es el rate limiting y por qué es importante en una API?

**Rate limiting** es una técnica que restringe la cantidad de peticiones que un cliente puede hacer a una API en un período de tiempo determinado.

### ¿Por qué es importante?

- **Protege contra abusos** (ej: ataques DDoS o bots).
- **Preserva recursos** del servidor.
- **Garantiza disponibilidad** para todos los usuarios.
- **Controla costos** en servicios de terceros (como APIs pagas por uso).

### Ejemplo de regla:
> “Máximo 100 solicitudes por minuto por IP”

### Métodos comunes de implementación:

- **Token Bucket**
- **Leaky Bucket**
- **Fixed Window**
- **Sliding Window**

### Herramientas y prácticas:
- Middleware (ej: `express-rate-limit` en Node.js)
- Gateways como NGINX, Kong, Amazon API Gateway

### Ejemplo de respuesta con limitación:
```http
HTTP/1.1 429 Too Many Requests
Retry-After: 60
{
  "error": "Rate limit exceeded. Try again in 60 seconds."
}
```
El rate limiting es esencial en APIs públicas o de alto tráfico.

## 10. ¿Qué es el rate limiting y por qué es importante en una API?
