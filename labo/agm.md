# Arbol generador minimo

## Kruskal

### Union-Find

## Prim

Es un algoritmo goloso que en cada paso agrega las aristas con peso minimo.

Pseudocodigo abstracto

```python
Prim(G)
    # si un nodo ya fue alcanzado por algun eje
    visitado[n] = {false, ..., false}

    s = elegir un nodo inicial (puede ser cualquiera de G)
    visitado[s] = true

    mientras que no esten todos los nodos visitados hacer
        (v, w) = eje de menor costo entre los que conectan a un nodo ya visitado v con un nodo no vistado w
        visitado[w] = true
```

Y como lo implementamos? Hay dos formas

- Basica: O(n^2)
  
  Conviene cuando el grafo es **denso** (m es O(n^2))

- Con cola de prioridad: O(m log n).
  
  Es necesario tener una representación del grafo con listas de adyacencia.

  Conviene cuando el grafo es **esparso** (ralo es un docente) (m es O(n))

### B

