# Teoria de la complejidad computacional

Un algoritmo es **eficiente** cuando su complejidad es polinomial

> No es siempre asi porque importan las constantes y el grado del polinomio.

El objetivo es clasificar los **problemas** segun su dificultad.

Un problema esta **bien resuelto** si se conocen algoritmos eficientes.

## Versiones de un problema de optimización $\Pi$

Dada una instancia I de un problema $\Pi$ con funcion objetivo f, hay diferentes
versiones:

- **optimizacion**: Encontrar una **solucion optima** del problema $\Pi$ para I
  (de valor maximo o minimo)

- **evaluacion**: Determinar el **valor** de una solucion optima de $\Pi$ para I.
- **localizacion**: Dado k, determinar una **solucion factible** tq $f(S) \leq k$.
- **decision**: Dado k, existe una solucion factible tq $f(S) \leq k$?
> en localizacion y decision, es $\leq$ para minimizacion y $\geq$ para
> maximizacion.

> Ejemplo: TSP
> - optimizacion: circ. hamiltoniano de longitud minima
> - evaluacion: determinar el valor de un circ de long minima.
> - localizacion: Dado k, determinar un circ. hamil. de long menor o igual a k
> - desicion: Dado k, existe circ. hamil. de long menor o igual a k?
> 
> Todo problema de optimizacion se puede transformar de esta manera

Hay cierta relacion entre las distintas versiones del mismo problema, la de
optimizacion suele ser la mas dificil, y la de decision suele ser la mas facil.

Para los que cumplen que el valor de una solucion es K, cuyo logaritmo esta
acotado por un polinomio en la medida de la entrada, la version de
**evaluacion** se puede resolver ejecutando una cantidad polinomial de veces la
version de decision, realizando busqueda binaria sobre K.

Para la mayoria, **las cuatro son equivalentes**, si existe un algoritmo
polinomial para una, existe para todas. El estudio entonces se realiza sobre los
**problemas de decisión**, lo cual permite uniformarlo.

## Instancias de un problema

Una **instancia** de un problema es una especificacion de sus parametros.

Los problemas de desicion $\Pi$ tienen asociado un conjunto de instancias
factibles $D_\Pi$, y un subconjunto $Y_\Pi$ de instancias con respuesta SI.

## Problema intratable

No existe algoritmo eficiente que lo resuelva.

- Requiere una respuesta de longitud exponencial. (ej: pedir todos los circ.
  hamiltonianos de longitud a lo sumo s, como la rta es exp el tiempo tambien)

- El problema es **indecidible** (ex. HALT)
- El problema es **decidible** pero no se conocen algoritmos poly que lo
  resuelven. No sabemos si son intratables o no.

## SAT - Problema de satisfabilidad

Dada una formula logica saber si le puedo dar valores a las variables de forma
tal que sea verdadera.

Un **literal** es una var booleana o su negacion.

Una clausula es una disyuncion de literales, y una formula esta en FNC (forma
normal conjuntiva) si es una conjuncion (y) de clausulas. Toda formula logica
puede ser expresada en FNC. Un Y de Oes

Una formula se dice **satisfacible** si existe al menos una asignacion de
valores de verdad a sus variables tq sea verdadera.

**Problema de satisfacibilidad (SAT)**: Dada una formula en FNC, ver si es
satisfacible.

## Modelos de computo

- Maquina de Turing
- Maquinas de Acceso Random - RAM

Ambos son polinomialmente equivalentes, se pueden simular con un costo poly.

### Maquina de turing

Ya lo deberia saber de logica esto

- MTD: Maquinas de turing deterministicas, no hay mas de una transicion posible
- MTND: Maquinas de turing no deterministicas, puedo tener o ninguna o mas de una transicion.
  Deja de ser una funcion, pasa a ser un mapeo multivaluado.

  La idea es que se ejecutan en paralelo las distintas alternativas.

  Si una copia acepta la entrada (para un estado final de SI), todas paran.

## Clases

### Clase P

Un problema de decision $\Pi$ pertenece a la clase P (polinomial) si existe una
MTD de complejidad poly que lo resuelve.

Ejemplos

- Grafo conexo
- Grafo bipartito
- AGM
- Camino minimo entre un par de nodos
- Matching maximo
- Grafo euleriano

### Clase NP

NP es polinomialmente no deterministico. Las instancias de $\Pi$ con respuesta
SI son reconocidoas por una MTND poly.

Equivalentemente, dada una instancia de $\Pi$ con respuesta SI se puede dar un
certificado de longitud poly que garantiza que la respuesta es SI y puedo
verificarlo en tiempo polinomial.

Ejemplos:

- Todos los problemas de P (teniendo en cuenta que $P \subseteq NP$)
- Clique maxima
- TSP
- Conjunto independiente maximo
- SAT
  > Ejemplo de cert: una valuacion, y se puede chequear en tiempo poly. Entonces
  > $SAT \in NP$

La mayoría de los problemas de optimización pertenecen a NP.

**Lema**: Si $\Pi$ es un problema de desición que pertenece a NP, puede ser
resuelto por un algoritmo *deterministico* (una MTD) en tiempo **exponencial**
respecto del tamaño de la entrada.

## Transformaciones polinomiales

Transformar instancias de problemas distintas de forma polinomial, manteniendo
la respuesta.

## Clase NP-completo

Es el centro de la teoría de la complejidad computacional. Intuitivamente,  agrupa
los problemas de desición **más difíciles** de NP.

Propiedades:

- No se conocen algoritmos polinomiales para resolverlos
- Si existe un algoritmo polinomial para uno, existe para todos.
  (si alguno es polinomial, todos lo son)

$\Pi$ es **NP-completo** si

1. $\Pi \in$ NP.
2. $\forall\ \Pi' \in NP,\ \Pi' \leq_p \Pi$

Cualquier $\Pi$ que cumpla 2. se lo llama **NP-dificil** (es al menos tan
dificil como todos los problemas de NP.)

### SAT es NP-completo

**Teorema (Cook)**: SAT es NP-completo.

Esta demostrado en forma directa, agarra instancias genericas y da una funcion
poly.

### Probar pertenencia a NP-completo

Si encontramos $\Pi_1 \leq_p \Pi$ tq $\Pi_1 \in\$ NP-hard, entonces por
transitividad $\Pi$ tambien. Y si $\Pi \in$ NP entonces esta en NP-completo.

Mientras mas problemas se que son NP-completos, mas puedo tomar como $\Pi_1$

#### Clique es NP-completo

> Dado un grafo G = (V, X) y k, G tiene una clique de tamaño mayor o igual a k?

- CLIQUE $\in$ NP.
- SAT $\leq_p$ CLIQUE:
  
  Queremos transformar una instancia de SAT a una de CLIQUE.

  Sea Is una instancia generica de SAT con Ks clausuas. Queremos definir una
  instancia generica de clique (un grafo y un k).

  Definimos $k_c = k_s$ y $G = (V, X)$ de la siguiente manera:

  - Para cada literal x en la clausula i, ponemos un vértice $(x, i) \in V$.
  - $((x, i), (y, j)) \in X$ sii $x \neq \neg y$ y $i \neq j$. Uno no es la
    negación de otro y son distintas clausulas.

    > dibujito en la clase
