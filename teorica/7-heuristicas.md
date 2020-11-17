# Heuristicas

## Optimización combinatoria

Dado un conj de soluciones valuadas con alguna función, buscamos la de **mejor**
valor. Puede ser mínimo o máximo.

El conjunto de soluciones tiene cardinal finito pero suele ser **muy grande**, y
aparecen en un montón de aplicaciones prácticas.

- función objetivo: valor de la solución
- restricciones: condiciones que debe cumplir una solucion para ser **factible**
    tsp: que sea circ hamiltoniano
    agm: que sea AG

Problema de optimización: Determinar una sol factible que minimice (o maximice)
el objetivo.

A las soluciones que minimizan la func objetivo las llamamos **soluciones optimas**
u **optimos globales**.

Por ej. en agm el valor es unico, pero puede haber mas de uno que tenga ese
valor.

Surge la necesidad de *algoritmos heuristicos* cuando el tiempo que se tarda es
demasiado.

## TSP

Dado un grafo G con longitudes asignadas a las aristas
$l : X \to \mathbb{R}^{\geq 0}$, queremos determinar un circuito hamiltoniano de long
minima. Encontrar $C^0$ tq

$$l(C^0) = min\{ l(C) \mid C \text{ es un circuito hamiltoniano de } G \}$$

Puedo suponer que el grafo es completo, ya que a las aristas que no esten le
puedo poner un peso muy grande ($\infty$).

No se conocen algoritmos polinomiales para resolverlo.

Si tenemos n vertices, hay $\frac{(n-1)!}{2}$ posibilidades.

## Algoritmos heuristicos

Me dan una solucion *razonable* (no necesariamente la mejor) en un tiempo
*razonable* util en la practica. Tambien puede servir para problemas dificiles
de modelar, que relajo restricciones.

No solo sirven para opt. comb. sino tambien para problemas de decision (si o
no). Todo problema de optimizacion tiene su version de desicion. Por ej en TSP,
dado un grafo y un $k \in \mathbb{N}$, ver si existe un circ. hamil. de peso
$\leq k$.

### Clasicas

- Construyen una sol inicial (de la nada)
- Aplican una busqueda local para mejorar la primera sol.

#### Heurística para grafo hamiltoniano (Póse)

Si dice que si, es verdad que la tiene. Si dice que no, puede ser que no tenga o
que tenga.

### Heuristicas constructivas



## Algoritmos aproximados

No necesariamente brindan una sol optima, pero a dif de las heurisitcas puedo
acotar lo mala que puede llegar a ser la sol que me devuelve.

Dado A un algoritmo, f func objetivo, y $x^A$ la solucion dado por a y $x^*$ una
sol optima, decimos que es un **algoritmo $\epsilon$-aproximado** con $\epsilon
\in \mathbb{R}_{>0}$ si para toda instancia se cumple

$$|f(x^A) - f(x^*)| \leq \epsilon |f(x^*)|$$

