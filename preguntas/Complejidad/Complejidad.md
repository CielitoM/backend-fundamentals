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