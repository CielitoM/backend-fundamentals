# Preguntas sobre Complejidad Algorítmica (Big O)

---

## 1. ¿Qué significa que un algoritmo tenga complejidad O(n)?

Un algoritmo O(n) significa que su tiempo de ejecución crece linealmente con respecto al tamaño de la entrada.

- Si la entrada tiene 10 elementos, hará aproximadamente 10 operaciones.
- Si tiene 1.000 elementos, hará 1.000 operaciones.

### Ejemplo:
```python
def buscar_elemento(lista, objetivo):
    for x in lista:
        if x == objetivo:
            return True
    return False
```

 ## 2. ¿Cuál es la complejidad de buscar un elemento en una lista vs. en un diccionario?

| Estructura  | Complejidad de búsqueda | Notación |
|-------------|--------------------------|----------|
| Lista       | Lineal                   | O(n)     |
| Diccionario | Constante promedio       | O(1)     |

**Nota:** En casos extremos de colisiones, un diccionario puede degradarse a `O(n)`, pero eso es raro con un buen sistema de *hashing*.

## 3. ¿Qué complejidad tiene recorrer una lista de listas (matriz) de n × m elementos?

La complejidad es `O(n × m)`, porque hay que iterar sobre cada fila y cada columna dentro de cada fila.

🧪 **Ejemplo:**

```python
def sumar_matriz(matriz):
    suma = 0
    for fila in matriz:
        for valor in fila:
            suma += valor
    return suma
```

## 4. ¿Cuál es la complejidad del siguiente fragmento de código?
```python
for i in range(n):
    for j in range(n):
        print(i, j)
```
Cada iteración externa se repite n veces, y dentro de ella hay otras n repeticiones → n × n.

## Pregunta 5: ¿Qué complejidad tiene una búsqueda binaria en una lista ordenada?
O(log n)
Porque en cada paso se reduce el tamaño del problema a la mitad.

```python
def busqueda_binaria(lista, objetivo):
    izquierda, derecha = 0, len(lista) - 1
    while izquierda <= derecha:
        medio = (izquierda + derecha) // 2
        if lista[medio] == objetivo:
            return medio
        elif lista[medio] < objetivo:
            izquierda = medio + 1
        else:
            derecha = medio - 1
    return -1
```


## 6. ¿Cuál es el impacto de la recursividad en la complejidad temporal?

- La recursividad puede ser poderosa, pero también costosa si no se optimiza.
- Muchas funciones recursivas tienen complejidad exponencial si recalculan subproblemas.

### Ejemplo clásico: Fibonacci

```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n - 2)
```

Complejidad: O(2ⁿ) → ineficiente para valores grandes.

Solución: memoización o programación dinámica
```python
def fib_dp(n):
    memo = [0, 1]
    for i in range(2, n+1):
        memo.append(memo[i-1] + memo[i-2])
    return memo[n]
```

Complejidad: O(n)


## 7. ¿Cuándo conviene usar una tabla hash (`dict`) en lugar de una lista?

Usa un **diccionario (tabla hash)** cuando:

- Necesitás búsquedas rápidas por clave → O(1) promedio.
- Hay necesidad de acceder a elementos por identificador.

Usa una **lista** cuando:

- El orden importa o es secuencial.
- Se necesita recorrer todos los elementos en orden.

### Ejemplo:

```python
# Búsqueda en lista (O(n))
for producto in productos:
    if producto.id == 5:
        return producto

# Búsqueda en dict (O(1))
productos_por_id = {p.id: p for p in productos}
return productos_por_id[5]
```


## 8. ¿Qué complejidad tienen las funciones `map`, `filter` y `reduce` en listas?

Todas estas funciones recorren la lista **una sola vez**, por lo que tienen complejidad **O(n)**.

- `map`: aplica una función a cada elemento → O(n)
- `filter`: evalúa una condición sobre cada elemento → O(n)
- `reduce`: combina elementos usando una operación acumulativa → O(n)

### Ejemplo en Python:
```python
nombres = ["Ana", "Luis", "Carlos"]

# map
mayusculas = list(map(str.upper, nombres))

# filter
largos = list(filter(lambda x: len(x) > 3, nombres))

# reduce
from functools import reduce
total_letras = reduce(lambda acc, x: acc + len(x), nombres, 0)
```

Nota:
Aunque O(n), estas funciones son muy eficientes porque están optimizadas internamente.


## 9. ¿Qué prefieres entre un algoritmo O(n log n) y uno O(n²), y por qué?

En general, un algoritmo **O(n log n)** es más eficiente que uno **O(n²)**, especialmente cuando `n` (el tamaño de la entrada) es grande.

### Comparación práctica:

| n     | n log n (base 2) | n²     |
|-------|------------------|--------|
| 10    | ~33              | 100    |
| 100   | ~664             | 10,000 |
| 1,000 | ~9,966           | 1,000,000 |

→ A medida que `n` crece, la diferencia se vuelve enorme.

### Ejemplo real:
- O(n²): burbuja, selección, inserción (ordenamiento lentos).
- O(n log n): mergesort, heapsort (ordenamiento eficientes).

> Incluso si O(n²) puede ser más simple de implementar, **en producción se prioriza la eficiencia y escalabilidad**.

Siempre que sea posible, **prefiere algoritmos con mejor complejidad asintótica**, especialmente en sistemas con muchos datos o usuarios concurrentes.

## 10. ¿Qué representa la notación Big O y por qué es importante en programación backend?

La **notación Big O** describe el **crecimiento del tiempo o espacio** que requiere un algoritmo en función del tamaño de entrada `n`. Es una forma de analizar la eficiencia algorítmica.

### ¿Por qué es importante?
- Permite comparar algoritmos de forma teórica.
- Ayuda a anticipar problemas de rendimiento en sistemas con muchos usuarios o datos.
- Es clave para tomar decisiones técnicas informadas.

### Ejemplos comunes:

| Complejidad | Nombre           | Ejemplo                          |
|-------------|------------------|----------------------------------|
| O(1)        | Constante        | Acceso a un elemento en un array |
| O(log n)    | Logarítmica      | Búsqueda binaria                 |
| O(n)        | Lineal           | Recorrer un arreglo              |
| O(n log n)  | Lineal logarítmica | Mergesort, Quicksort (mejor caso) |
| O(n²)       | Cuadrática       | Burbujas, selección, inserción   |

### Ejemplo en código:

```python
# O(n): recorre una lista
def suma(lista):
    total = 0
    for num in lista:
        total += num
    return total
```

### Consideración:

Big O no mide tiempos reales, sino crecimiento relativo. Depende del peor caso (salvo que se aclare lo contrario).

> Dominar Big O permite escribir código más escalable y tomar mejores decisiones en el diseño backend.