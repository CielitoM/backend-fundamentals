# Pr..eguntas sobre Complejidad Algorítmica (Big O)

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

## 11. ¿Cuál es la complejidad del siguiente código y por qué?

```python
def buscar_pares(lista):
    for i in lista:
        for j in lista:
            if (i + j) % 2 == 0:
                print(i, j)
```

El código tiene dos bucles anidados que recorren la lista completa.

Si la lista tiene n elementos, el número total de combinaciones es n * n → O(n²).


### Análisis:

| Línea                 | Operación                         | Complejidad |
|----------------------|-----------------------------------|-------------|
| `for i in lista:`    | Recorre `n` elementos             | O(n)        |
| `for j in lista:`    | Anidado, se ejecuta por cada `i`  | O(n)        |
| `print(i, j)`        | Operación constante               | O(1)        |


→ Complejidad total: O(n²)

### ¿Cómo podría optimizarse?

Si no necesitas combinaciones repetidas ((i,j) y (j,i)), puedes usar for j in lista[i+1:] para reducir a aproximadamente la mitad las iteraciones.

Pero la complejidad seguiría siendo O(n²), solo con un mejor coeficiente.


> Saber leer y analizar código te permite anticipar cuellos de botella en tiempo de ejecución.

## 12. ¿Cuál es la complejidad de las operaciones básicas en un diccionario (hashmap) y por qué?

Los **diccionarios** (también llamados **hashmaps** o **tablas hash**) permiten almacenar y acceder a datos por clave. En la mayoría de los lenguajes modernos (como Python, JavaScript, Java), están implementados con **hash tables**.

### Complejidades comunes:

| Operación         | Complejidad Promedio | Complejidad Peor Caso |
|-------------------|----------------------|------------------------|
| Insertar (`put`)  | O(1)                 | O(n) (en caso de colisiones extremas) |
| Acceder (`get`)   | O(1)                 | O(n)                   |
| Eliminar (`del`)  | O(1)                 | O(n)                   |

> En la práctica, con buenas funciones de hash y mecanismos de resolución de colisiones, el rendimiento se mantiene cercano a **O(1)**.

### Ejemplo en Python:

```python
usuarios = {"ana": 25, "juan": 30}
usuarios["ana"]  # acceso en O(1)
usuarios["carlos"] = 22  # inserción en O(1)
```

### Consideraciones:

La eficiencia depende de la función hash y la implementación interna.

En situaciones donde hay muchas colisiones, el rendimiento puede degradarse.


> Los diccionarios son una de las estructuras más eficientes para búsquedas, pero no siempre son la mejor opción si se necesita orden o evitar colisiones.

## 13 ¿Qué es la complejidad espacial y cómo se diferencia de la complejidad temporal?

La **complejidad espacial** mide cuánta **memoria adicional** necesita un algoritmo para ejecutarse, en función del tamaño de la entrada.

### Comparación:

| Tipo de complejidad | Qué mide                    | Ejemplo de unidad         |
|---------------------|-----------------------------|---------------------------|
| Temporal            | Tiempo de ejecución         | Operaciones / pasos       |
| Espacial            | Memoria utilizada           | Variables, arreglos, etc. |

### Ejemplo simple:

```python
def suma(lista):
    total = 0      # O(1)
    for num in lista:
        total += num
    return total
```

Complejidad temporal: O(n) (por el bucle)

Complejidad espacial: O(1) (solo una variable total)


### Ejemplo con mayor uso de memoria:

```python
def duplicar(lista):
    nueva = []
    for x in lista:
        nueva.append(x * 2)
    return nueva
```

Aquí, la complejidad espacial es O(n), porque se crea una nueva lista del mismo tamaño que la original.


### ¿Por qué es importante?

En sistemas con memoria limitada (embedded, móviles).

Cuando se procesan grandes volúmenes de datos.

Para optimizar algoritmos más allá del tiempo.


> Una solución rápida que consume demasiada memoria puede no ser viable en producción. El equilibrio entre tiempo y espacio es clave.

## 14. ¿Cuál es la complejidad temporal de los algoritmos de ordenamiento más comunes?

A continuación se muestra una tabla con la complejidad temporal de algunos algoritmos de ordenamiento conocidos:

| Algoritmo          | Mejor caso       | Promedio         | Peor caso          | Estable |
|--------------------|------------------|------------------|--------------------|---------|
| Bubble Sort        | O(n)             | O(n²)            | O(n²)              | Sí      |
| Insertion Sort     | O(n)             | O(n²)            | O(n²)              | Sí      |
| Selection Sort     | O(n²)            | O(n²)            | O(n²)              | No      |
| Merge Sort         | O(n log n)       | O(n log n)       | O(n log n)         | Sí      |
| Quick Sort         | O(n log n)       | O(n log n)       | O(n²)              | No      |
| Heap Sort          | O(n log n)       | O(n log n)       | O(n log n)         | No      |

### Tip:
- **Estabilidad** significa que los elementos con valores iguales mantienen su orden relativo original.
- QuickSort es muy rápido en la práctica, pero su peor caso puede ser problemático si no se elige bien el pivote.
- MergeSort es preferido cuando se necesita un algoritmo estable con buen rendimiento garantizado.

## 15. ¿Cuál es la complejidad temporal de acceso, inserción y eliminación en distintas estructuras de datos?


| Estructura de datos      | Acceso     | Inserción | Eliminación |
|--------------------------|------------|-----------|-------------|
| Array (estático)         | O(1)       | O(n)      | O(n)        |
| Lista enlazada simple    | O(n)       | O(1)      | O(1) (al inicio) |
| Pila (stack)             | O(n)       | O(1)      | O(1)        |
| Cola (queue)             | O(n)       | O(1)      | O(1)        |
| Hash Table               | O(1)       | O(1)      | O(1)        |
| Árbol binario balanceado (AVL, Red-Black) | O(log n) | O(log n) | O(log n) |

### Tip:
- El array permite acceso directo (índice), pero no es eficiente para insertar o borrar elementos.
- Las hash tables ofrecen acceso e inserción promedio en tiempo constante, pero pueden degradarse a O(n) en caso de muchas colisiones.
- Árboles balanceados garantizan operaciones eficientes incluso en el peor caso.

## 16. ¿Cuál es la complejidad temporal del siguiente algoritmo recursivo?

```python
def f(n):
    if n <= 1:
        return 1
    return f(n - 1) + f(n - 1)
```

Este algoritmo llama a f(n-1) dos veces en cada nivel de recursión. Podemos expresar su complejidad como:

T(n) = 2 * T(n - 1) + O(1)

La solución a esta recurrencia es:

T(n) = O(2^n)

### Explicación:

En cada nivel, el número de llamadas se duplica.

La altura del árbol de recursión es n, y hay 2^n llamadas al final.


Esta es una recursión exponencial, muy costosa en tiempo.

> Optimización posible: usar memoization o programación dinámica para reducir a O(n).


## 17. Dado un array de enteros, ¿cómo encontrar la subcadena contigua con la suma más alta en el menor tiempo posible?

Ejemplo:
```python
nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
```

Este es un problema clásico de optimización conocido como el máximo subarreglo contiguo (Maximum Subarray Problem), resuelto eficientemente con el algoritmo de Kadane.

### Enfoque:

def max_subarray(nums):
    max_actual = max_total = nums[0]
    for n in nums[1:]:
        max_actual = max(n, max_actual + n)
        max_total = max(max_total, max_actual)
    return max_total

### Complejidad:

Tiempo: O(n)

Espacio: O(1)


### Explicación:

Se recorre el array una sola vez.

Se acumula la suma máxima posible terminando en cada posición.

Si una suma acumulada es menor que el número actual, se reinicia desde allí.

Este algoritmo evita cálculos innecesarios como en la solución de fuerza bruta O(n²).

## 18. ¿Qué significa que un algoritmo tenga complejidad **tiempo lineal** pero **espacio logarítmico**?

Significa que:

- El **tiempo de ejecución** del algoritmo crece proporcionalmente al tamaño de la entrada (O(n)).
- El **uso de memoria adicional** (espacio auxiliar) crece en proporción al logaritmo del tamaño de la entrada (O(log n)).

### Ejemplo:

```python
def recursivo_log(n):
    if n <= 1:
        return
    recursivo_log(n // 2)

def linear_scan(arr):
    for x in arr:
        print(x)
```

linear_scan tiene tiempo O(n) y espacio O(1).

recursivo_log tiene tiempo O(log n) y usa O(log n) espacio por la pila de llamadas recursivas.


Si combinamos un recorrido lineal con llamadas recursivas logarítmicas, podríamos tener un algoritmo con tiempo O(n) y espacio O(log n).


> Una buena comprensión de cómo varían tiempo y espacio según la estrategia (recursión, iteración, estructuras auxiliares) es clave para optimizar software backend eficiente.


## Analiza la complejidad temporal y espacial del siguiente algoritmo, e indica si puede optimizarse: 

```python
def buscar_suma(lista, objetivo):
    for i in range(len(lista)):
        for j in range(len(lista)):
            if lista[i] + lista[j] == objetivo:
                return True
    return False
```

### 1. Complejidad temporal

El algoritmo usa dos bucles anidados de tamaño n, por lo que:

Tiempo: O(n²) en el peor de los casos.


### 2. Complejidad espacial

Usa solo variables auxiliares (i, j), por lo que:

Espacio: O(1).


### 3. Posible optimización

Usar una estructura de datos como set para almacenar los elementos ya vistos y verificar complementos en tiempo constante:

Nuevo tiempo: O(n)

Nuevo espacio: O(n)

## 19. Analiza la complejidad temporal y espacial del algoritmo de búsqueda binaria y explica en qué condiciones es más eficiente que la búsqueda lineal.

1. **Complejidad temporal:**
   - Búsqueda binaria: O(log n) en el mejor, promedio y peor caso.
   - Búsqueda lineal: O(n) en el peor caso.

2. **Complejidad espacial:**
   - Búsqueda binaria: O(1) si es iterativa, O(log n) si es recursiva (por el stack).
   - Búsqueda lineal: O(1).

3. **Condiciones de eficiencia:**
   - La búsqueda binaria requiere que la colección esté ordenada.
   - Es más eficiente que la búsqueda lineal cuando el tamaño de la colección es grande y ya está ordenada.

4. **Ejemplo de comparación:**
   - En una lista ordenada de un millón de elementos:
     - Búsqueda lineal → hasta 1,000,000 comparaciones.
     - Búsqueda binaria → máximo ~20 comparaciones.

## 20. Determina la complejidad temporal y espacial de un algoritmo de ordenamiento por burbuja (Bubble Sort) y compáralo con el de QuickSort. Explica en qué escenarios podría justificarse el uso de Bubble Sort.

1. **Complejidad temporal:**
   - **Bubble Sort:**
     - Mejor caso: O(n) (cuando la lista ya está ordenada y se optimiza el algoritmo).
     - Promedio y peor caso: O(n²).
   - **QuickSort:**
     - Mejor y promedio: O(n log n).
     - Peor caso: O(n²) (cuando siempre se elige un pivote extremo en listas ya ordenadas).

2. **Complejidad espacial:**
   - **Bubble Sort:** O(1) (in-place).
   - **QuickSort:** O(log n) por la pila de recursión.

3. **Escenarios donde Bubble Sort puede ser útil:**
   - Listas muy pequeñas.
   - Cuando la lista ya está casi ordenada y se usa la versión optimizada.
   - En entornos educativos para explicar conceptos básicos de ordenamiento.

4. **Ejemplo comparativo:**
   - Lista de 10,000 elementos aleatorios:
     - Bubble Sort → ~100,000,000 comparaciones.
     - QuickSort → ~132,877 comparaciones en promedio.

> Aunque Bubble Sort es ineficiente para grandes volúmenes de datos, su simplicidad y facilidad de implementación lo hacen útil en casos muy específicos.