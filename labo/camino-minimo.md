# Camino minimo

- Problema: Dado un grafo $G = (V, E)$ con pesos en las aristas $p: E \to R$ y
  dos verts $v_0, v_1 \in V$, hallar el camino $v_0-v_1$ de minimo peso. Puede
  haber varios.

Variantes

- 1 - n: $v_0$ fijo y $v_1$ varia (único origen, múltiples destinos)
- n - n: Ambos varian (múltiples orígenes, múltiples destinos)
- Pesos no negativos: $p(e) \geq 0$ para toda arista $e \in E$

## Grafos en c++

Representación de grafos por listas de adyacencia

```cpp
#include <vector>
using namespace std;

typedef int Vertice;
typedef int Peso;

struct Vecino {
    Vertice dst;
    Peso peso;
    Vecino(Vertice d, Peso p): dst(d), peso(p){}
}

// Representacion por lista de adyacencia.
typedef vector<vector<Vecino>> Grafo;
```

## 1 - n

### Dijkstra

Pesos no negativos.




## n - n