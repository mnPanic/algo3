# Técnicas de diseño de algoritmos

## Fuerza bruta

Idea: Analizar todas las posibilidades y quedarme con las que me interesan.

- Faciles de inventar e implementar
- La tecnica siempre permite hacer un algoritmo que obtenga lo que quiero
  obtener.
- Pero generalmente son **muy ineficientes**.

En la practica solo me sirve cuando lo corro sobre instancias muy chicas, o para
asegurarme que otro algoritmo es correcto comparandolos con instancias pequeñas.

### Problemas de las n reinas

Problema: Hallar todas las formas posibles de colocar *n* reinas en un tablero
de ajedrez de n x n, de forma tal que ninguna amenace a otra (misma col, misma
fila, o misma diagonal)

- Solucion por FB: Hallar *todas* las formas posibles de colocar *n* reinas en
  un tablero de n x n y luego seleccionar las que satisfagan las restricciones.

  Cuales son todas? De $n^2$ elijo n, es el combinatorio
  
  $${n^2 \choose n} = \frac{n^2!}{(n^2 - n)!n!}$$

  Pero como la mayoria de las configs no cumplen las reestricciones, trabajamos
  de mas y resulta muy ineficiente

## Algoritmos golosos

Idea: Construir una solucion **paso a paso**. Comienzo por una solucion vacia y
en cada paso agrego la mejor alternativa posible *localmente* (en ese momento,
segun alguna funcion de seleccion) sin considerar las implicancias de esta
seleccion.

Goloso porque se come lo mejor en el momento sin importar el futuro.

En cada etapa se toma la decision que aprece mejor basandose en la info
disponible en ese momento, sin importar las consecuencias futuras. Una decision
tomada **nunca** es revisada y no se evaluan alternativas.

- Faciles de inventar e implementar y generalmente eficientes.
- Pero no para todos los problemas *funcionan* bien. No siempre hay algoritmos
  golosos correctos.
  - Proporcionan *heuristicas* sencillas para problemas de optimizacion.
    Una heuristica es un algoritmo que me da soluciones buenas pero no
    necesariamente las mejores. En general son razonables, pero sub-optimas.

Es crucial definir bien la **funcion de seleccion**

### El problema de la mochila (continuo)

**Entrada**:

- Capacidad $C \in R_+$ de la mochila (peso maximo)
- Cantidad $n \in N$ de objetos que podria llegar a poner.
- Peso $p_i \in R_+$ del objeto i
- Beneficio $b_i \in R_+$ del objeto i

**Problema**: Determinar que objetos meto en la mochila, sin pasarse del peso
maximo, para **maximizar** el beneficio total entre los objetos seleccionados.
Es una version mas simple porque suponemos que podemos poner **parte** de un
objeto en la mochila (continuo).

Si bien parecen similares, computacionalmente son distintos. En continuo un
goloso funciona, pero en el caso discreto tiene una complejidad muy mala. No se
conocen algoritmos polinomiales.

**Algoritmo**: Mientras no se haya excedido el peso, agrego el objeto $i$ que
tenga mejor resultado de la funcion de seleccion

- Tenga el mayor beneficio $b_i$
- Tenga menor peso $p_i$
- Tenga mayor beneficio por unidad de peso $\frac{b_i}{p_i}$
  > Se puede demostrar que esta es una solucion optima. Da la mejor solucion
  > posible.

> Ejemplo en el video y en el apunte

**Complejidad**? O(nlogn)? chequear

Y si fuera **discreto**? Pasa a ser un problema de decision y la complejidad
cambia drasticamente.

### El problema del cambio

**Problema**: Le tengo que dar el vuelto a un cliente usando el minimo numero de
monedas posibles, de 1, 5, 10 y 25 centavos. Suponiendo que las monedas no se
acaban. Que sean estos valores y no otros es *decisivo* en el problema.
Por ej: si el monto es $0,69 tengo que entregar 8 monedas: 2 de 25, una de 10,
una de 5 y cuatro de 1.

**Algoritmo**: voy agarrando la moneda de mayor denominacion que puedo. Agarro
tantas de 25 como me entren, a lo que me quede de 10, y asi.

Seleccionar la moneda de mayor valor que no exceda la cantidad restante por
devolver, agregar esta moneda a la lista de la solucion, y sustraer la cantidad
correspondiente a la cnatidad que resta por devolver hasta que sea 0.

> Como tengo la de 1, siempre le voy a poder dar vuelto
> No es un parametro los valores de las monedas que tengo disponibles.

```text
darCambio(cambio)
    in: cambio in N
    out: M conjunto de enteros

    suma = 0
    M = {}

    while suma < cambio:
        proxima = mas_grande(cambio, suma)
        M = M U {proxima}
        suma += proxima

    return M
```

Se puede demostrar que *funciona*: da la menor cantidad de monedas posibles
para devolver el cambio siempre y cuanto **sean esas las denominaciones**.

Imaginando que tambien tenemos de 12, no encontraria una solucion optima, por ej
para devolver 21, devolveria `12 5 1 1 1 1` en vez de la optima, `10 10 1`.
Aqui **la tecnica no funciona**

El algoritmo es goloso porque selecciona la moneda de mayor valor osible, sin
rpeocuparse que esto puede llevar a una mala solucion, y nunca modifica una sol
tomada.

### Tiempo de espera total en un sistema

**Problema**: Un servidor tiene $n$ clientes para atender, en el orden que
quiera. Cada uno tiene un tiempo de atencion necesario $t_i$. El objetivo es
encontrar en que secuencia atenderlos tal que se minimicen los tiempos de espera
de los clientes. Que se minimice *la suma de los tiempos de espera* de los
clientes.

La idea mas natural es atender primero al que requiera menos tiempo de atencion,
y asi.

**Algoritmo goloso**: En cada paso atender al cliente pendiente que tenga menor
tiempo de atencion.

Los ordeno segun su tiempo de atencion y los atiendo en ese orden.

**Complejidad**: Ordenarlos. O(n lgn)

Produce la **solucion optima!**

## Algoritmos recursivos

> La venimos viendo desde el taller de algebra.

**Idea**: Un algoritmo recursivo se define en terminos de si mismo. En su cuerpo
aparece una aplicacion de la misma funcion.

- Debe haber por lo menos un **caso base** elemental. Un caso no recursivo que
  se resuelve de otra forma.

- Las llamadas recursivas deben llamarse sobre parametros **mas chicos** que los
  iniciales. La *pequeñez* se mide en cada problema de una forma distinta.

- Cada llamada se debe *acercar mas* al caso base, que al ser alcanzado termina
  la recursion.

> Aca no tome mucha nota porque son cosas que vimos 1000 veces

### Complejidad

Se pueden describir mediante ecuaciones de recurrencia, las cuales suelen ser
faciles de escribir pero despues es necesario encontrar una formula cerrada.

$$ T(n) = n \times T(n-1)$$

### Correctitud de algoritmos recursivos

Para demostrar la correctitud, tenemos que demostrar

- Que termina (es decir que es un algoritmo)
  - Los procesos repetitivos terminan cuando se aplican a instancias que cumplen
    con la precondicion.

- Que cumple la especificacion
  - Si el estado inicial satisface la precond. entonces el estado final cumple
    la postcond. Frecuentemente esto se demuestra por induccion.

> Un ejemplo de como hacer esto para un `cuadrado` raro en el apunte/diapos

## Divide and conquer (dividir y conquistar)

Consiste en

- Si la instancia I es chica, entonces uso un algoritmo *adhoc* para el problema
- Sino,
  - **Divido** I en sub instancias $I_1, I_2, ..., I_r$ mas chicas
  - Resuelvo **recursivamente** las $r$ sub-instancias.
  - **Combino** las soluciones para las $r$ sub-instancias para obtener una
    solucion para la instancia original $I$.

Esquema general

```text
d&c(x)
    in: x
    out: y

    if x es suficientemente facil
        y = calcular adhoc
    else
        descomponer x en instancias mas chicas x_1, ..., x_r
        for i = 1 .. r
            y_i = d&c(x_i)

        y = combine los Y-i

    return y
```

Para que sea eficiente (de buenas complejidades)

- La cantidad de instancias no deberia ser muy grande, 2, 3 etc. y tiene que ser
  un numero independiente de la instancia.

- Los tamaños de cada subproblema deberian ser de tamaño parecido, y que no haga
  falta resolver dos veces lo mismo.

### Complejidad de D&Q

Son algoritmos recursivos, por lo tanto hacemos el mismo razonamiento de antes.
Generalmente los casos bases son O(1), y para los casos recursivos:

- Cantidad de llamadas recursivas: $r$
- Medida de cada subproblema $n/b$ para alguna constante $b$
- Tiempo requerido para dividir y combinar una instancia de tamaño $n$, $g(n)$

La complejidad entonces es

- si n es caso base

  $$ T(n) = c $$

- si n es caso recursivo

  $$ T(n) = r \times T(\frac{n}{b}) + g(n)$$

#### Teorema Maestro

La usamos demostrada en una version simplificada

Si existe un entero k tal que g(n) es $O(n^k)$, se puede ver que

- si $r < b^k$, $O(n^k)$
- si $r = b^k$, $O(n^k \log n)$
- si $r > b^k$, $O(n^{\log_{b} r})$

### D&Q - Busqueda binaria

**Problema**: Decidir si un elem x se encuentra en un arreglo de n elementos A,
ordenado de forma creciente.

**Algoritmo D&Q**:

- Decido si esta en la primera mitad o la segunda mitad del arreglo. Basta
  comparar x con el elemento de la posicion media del arreglo.

- Si x es menor al del medio, esta en la primera mitad, sino en la segunda.
  
```text
busqueda_binaria(A, x)
    in: array A, valor x
    out: True si x esta en A, False sino.

    if dim(A) = 1
        return x == A[0]

    k = dim(A) / 2

    si x <= A[k-1]
        return busqueda_binaria(A[0...k-1], x)
    else
        return busqueda_binaria(A[k...dim(A)-1], x)
```

**Complejidad**: con n > 1,

$$ T(n) = T(n/2) + g(n)$$

Donde $g(n)$ es $O(1) = O(n^0)$

- k = 0, r = 1, b = 2

Como $b^k = b^0 = 1 = r$, entonces entra por el segundo caso del teorema maestro
y la solucion a la recurrencia $T(n)$ es $O(n^k \log n) = O(\log n)$

### D&Q - Mergesort

**Problema**: Ordenar un arreglo $A$ de n elementos.

**Algoritmo de D&C**:

- Si n es chico, ordenar por cualquier metodo
- Si n es grande,
  - $A_1$ := primera mitad de $A$.
  - $A_2$ := segunda mitad de $A$
  - Ordenar recursivamente $A_1$ y $A_2$
  - Combinar $A_1$ y $A_2$ para obtener los elementos de $A$ ordenados
    (merge de arreglos ordenados, $O(n)$)

> Algoritmo en el apunte

Entonces la formula de recurrencia es

$$T(n) = 2 \times T(\frac{n}{2}) + g(n)$$

Donde $g(n)$ es $O(n)$

Aplicando teorema maestro con r = 2, b = 2, k = 1, $r = b^k$ y $T(n)$ es $O(n
\log n)$

## Backgracking

**Idea**: Recorrer sistematicamente de una forma ordenada todas las posibles
configuraciones del espacio de soluciones.

A las configuraciones las llamaremos **soluciones válidas**

Puede usarse para resolver problemas de *factibilidad* (en donde se quiere
encontrar cualquier solución válida o todas ellas) o problemas de optimización
(donde de todas las soluciones válidas queremos la **mejor**)

**Backtracking** es una modificacion de fuerza bruta que aprovecha propiedades
del problema para evitar analizar *todas* las soluciones. Entonces para ver que
es correcto tenemos que estar seguros de que no evite ver soluciones que me
interesan.

- Se usa un **vector** $a = (a_1, ..., a_n)$ para representar una solucion
candidata, donde cada $a_i$ pertenece a un dominio/conjunto finito $A_i$.
Entonces el espacio donde viven mis soluciones es el producto cartesiano $A_i
\times ... \times A_n$

- En cada paso extiendo las soluciones parciales agregando un elemento mas
- Si el conjunto de soluciones sucesoras es vacio, esa rama no la continuo
  explorando.

> Mas detalle en apunte

```text
BT(a, k)
    in: a = (a_1, ..., a_k) sol parcial
    out: se procesan todas las sol validas

    if k == n + 1
        procesar(a)
    else
        for a' in Sucesores(a, k)
            BT(a', k+1)
```

### Backtracking - Problema de las 8 reinas

Es como el de [n reinas](#problemas-de-las-n-reinas) pero con 8.

**Problema**: Ubicar 8 reinas en un tablero de 8x8 sin que se amenacen entre si

Con fuerza bruta,

$${64 \choose 8} = 442616536$$

Pero sabemos que cada fila debe tener exactamente una reina, entonces una sol
puede estar representada por solamente encontrar en que columna poner a cada
una, $(a_1, ..., a_8)$ donde $a_i \in {1, ..., 8}$ indica la columna en la que
esta la reina de la fila i-esima.

Y una solucion parcial puede estar representada por $(a_1, ..., a_k), k \leq 8$

Si ahora hicieramos fuerza bruta, tenemos $8^8$ combinaciones, pero cada columna
debe tener exactamente una reina. Entonces pasa a ser $8!$. Pero no todas
seransoluciones de nuestro problema, ya que falta verificar que no haya reinas
que se amenacen por estar en la misma diagonal.

Y se cumple la prop domino: Si tengo una sol parcial que ya no cumple lo que
quiero, ya tiene reinas que se amenazan, entonces toda extension de ella tambien
va a tener reinas que se amenazan. Por lo tanto es correcto podar una sol
parcial que tenga reinas que se amenazan.

**Construir los sucesores de forma inteligente**: Sin generar con reinas que se
amenazan. La nueva reina k-esima (de la fila k)

- no puede estar en ninguna de las columnas anteriores (las filas estan
  aseguradas con la representacion elegida)

- no puede estar en ninguna de las diagonales donde esten las k anteriores

Para esto, basta con

$$ \text{Sucesores(a, k)} = \{(a, a_k) : a_k \in \{1, ..., 8\}, a_k - a_j \notin \{k - j, 0,
j - k\} \forall j \in \{1, ..., k - 1\}\}$$

Para todos los anteriores, ver que $a_k - a_j$ no es

- 0: no son iguales, no estaran en la misma columna
- k - j o j - k: no comparten diagonal

Con esto ya se puede construir el algoritmo.

### Backtracking - Suma de subconjuntos

> Apunte / diapos

## Programación Dinámica

**Idea**: Se divide en subproblemas mas chicos, y una vez resueltos se combinan
para generar la solucion del problema original. En ese sentido es parecido a D&Q

Se aplica típicamente a problemas de optimización combinatoria pero tambien es
adecuada para problemas de *naturaleza* recursiva.

Es útil para problemas que fallan con D&Q.

- Instancias de tamaño desparejo
- Reutilizar subproblemas sobre la misma subinstancia

Características

- Evita hacer llamadas recursivas guardando resultados calculados, para no tener
  que recalcularlos.
- Es **bottom up** y no recursivo: arranca de los problemas mas chicos y va
  construyendo mas grandes hasta llegar a lo que quiero. No es recursivo sino
  iterativo.
- Lo malo es que no todo problema se puede resolver de esta manera, tiene que
  cumplir un *principio* o propiedad.

### Principio de optimalidad de Bellman

Un problema de optimización satisface este principio si en una sol. optima cada
subsolución es optima a su vez de ese subproblema correspondiente. Dada una
secuencia ótima de decisiones, toda subsecuencia de ella es optima.

Si miramos una subsolución de la optima, debe ser solución del subproblema
asociado a esa subsolución.

Este principio es **condición necesaria** para poder usar la técnica de
programación dinámica.

#### Bellman - Camino mínimo

El camino mínimo (sin distancias negativas) entre dos ciudades cumple con el
principio:

- Si el camino mas corto para llegar de A a B pasa por C, para transitar el
  camino mas corto de A a B vamos a tener que hacer el mas corto de A a C y de C
  a B.

- Entonces la solucion optima se construye *anexando* soluciones optimas de
  problemas mas chicos. Por lo tanto cumple el principio de Bellman.

Esto garantiza que podemos construir la sol. optima de una instancia *juntando*
de alguna manera sol optimas de instancias mas chicas.

#### Bellman - Camino máximo

Camino máximo entre un par de ciudades sin repeticiones. Suponiendo que las
ciudades estan tipo en *token ring*.

> No se cumple, ver apunte

### Programacion Dinamica - Multiplicacion de n matrices

> ver apunte

### Subsecuencia común más larga

> apunte
