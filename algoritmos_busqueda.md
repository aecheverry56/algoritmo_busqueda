# Algoritmos de búsqueda

## Introducción

Este documento presenta siete algoritmos y estructuras de datos usados para buscar elementos. En cada caso se explica qué hacen, cuánto cuestan y cuándo conviene usarlos.

En las tablas, **n** representa la cantidad de elementos. La notación **O(·)** describe cómo crece el trabajo cuando aumenta el tamaño de los datos. Para los algoritmos de búsqueda, el espacio indicado es auxiliar; para el BST y la Hash Table, se refiere al espacio necesario para almacenar la estructura.

## Resumen de eficiencia

| Algoritmo o estructura | ¿Requiere datos ordenados? | Tiempo de búsqueda | Espacio |
|---|---|---:|---:|
| Búsqueda lineal | No | Mejor: O(1); promedio y peor caso: O(n) | O(1) |
| Búsqueda binaria | Sí | Mejor: O(1); promedio y peor caso: O(log n) | O(1) iterativa; O(log n) recursiva |
| Jump Search | Sí | Mejor: O(1); promedio y peor caso: O(√n) | O(1) |
| Interpolation Search | Sí, con claves numéricas | Mejor: O(1); promedio: O(log log n) si la distribución es uniforme; peor caso: O(n) | O(1) |
| Exponential Search | Sí | Mejor: O(1); O(log i) si el elemento está en la posición i; peor caso: O(log n) | O(1) iterativa |
| Árbol binario de búsqueda balanceado (BST) | Mantiene las claves ordenadas en el árbol | Buscar, insertar y eliminar: O(log n) | O(n) para almacenar el árbol |
| Hash Table (tabla hash) | No | Promedio esperado: O(1); peor caso: O(n) | O(n) |

> Las eficiencias promedio de Interpolation Search y Hash Table dependen de sus supuestos: distribución de claves aproximadamente uniforme en el primer caso, y una función hash y resolución de colisiones adecuadas en el segundo.

---

## 1. Búsqueda lineal

### Definición

Recorre la colección desde el inicio y compara cada elemento con el valor buscado hasta encontrarlo o llegar al final. También se conoce como búsqueda secuencial.

### Eficiencia

- **Mejor caso:** O(1), si el primer elemento coincide.
- **Promedio:** O(n), porque puede revisar una parte importante de la colección.
- **Peor caso:** O(n), si el elemento está al final o no existe.
- **Espacio auxiliar:** O(1).

No necesita que los datos estén ordenados.

### Casos de uso

- Buscar en una lista pequeña, desordenada o que cambia con frecuencia.
- Revisar datos cuando no se justifica ordenarlos o construir una estructura adicional.
- Comprobar si un elemento aparece en una colección sencilla.

**Ejemplo:** para buscar `23` en `[8, 14, 23, 31]`, se comparan `8`, luego `14` y finalmente `23`.

---

## 2. Búsqueda binaria

### Definición

Busca en una secuencia ordenada. Compara el objetivo con el elemento del medio; si no coincide, descarta la mitad que no puede contenerlo. Repite el proceso en la mitad restante.

### Eficiencia

- **Mejor caso:** O(1), si el elemento del medio es el buscado.
- **Promedio y peor caso:** O(log n), porque cada comparación reduce aproximadamente a la mitad el intervalo pendiente.
- **Espacio auxiliar:** O(1) en una implementación iterativa; O(log n) en una implementación recursiva debido a las llamadas.

**Requisito:** los elementos deben estar ordenados y la colección debe permitir acceder eficientemente a una posición intermedia, como ocurre en un arreglo.

### Casos de uso

- Consultar repetidamente un arreglo ordenado.
- Encontrar un nombre, código o valor en una lista ordenada.
- Ubicar límites o posiciones, por ejemplo, el primer valor que cumple una condición monotónica.

**Ejemplo:** en `[3, 7, 12, 18, 23, 31, 40]`, para buscar `23` se compara primero con `18`; como `23` es mayor, se continúa en la mitad derecha.

---

## 3. Jump Search (búsqueda por saltos)

### Definición

Busca en una secuencia ordenada saltando bloques de tamaño aproximado √n. Cuando encuentra un bloque cuyo último elemento es igual o mayor que el objetivo, revisa ese bloque elemento por elemento.

### Eficiencia

- **Mejor caso:** O(1), si el objetivo está en la primera posición que se examina.
- **Promedio y peor caso:** O(√n), usando bloques de tamaño √n. Se realizan aproximadamente n/√n saltos y, como máximo, √n comparaciones dentro del bloque final.
- **Espacio auxiliar:** O(1).

**Requisito:** datos ordenados y acceso a posiciones por índice.

### Casos de uso

- Buscar en arreglos ordenados cuando se quiere usar saltos y luego un recorrido local corto.
- Almacenamientos ordenados en los que resulta conveniente saltar por bloques y luego hacer un recorrido secuencial corto. En arreglos habituales con acceso aleatorio, la búsqueda binaria normalmente hace menos comparaciones.

**Ejemplo:** con bloques de tamaño 3, se revisan primero las posiciones 2, 5, 8, etc. Al hallar un valor que supera el objetivo, se busca linealmente en el bloque anterior.

---

## 4. Interpolation Search (búsqueda por interpolación)

### Definición

Busca en una secuencia ordenada de claves numéricas. En lugar de consultar siempre la posición del medio, estima dónde debería estar el objetivo según su valor y los valores de los extremos del intervalo. Por ejemplo, si el objetivo está cerca del valor máximo, consulta una posición cercana al final.

### Eficiencia

- **Mejor caso:** O(1), si la primera estimación encuentra el objetivo.
- **Promedio esperado:** O(log log n) cuando los valores están distribuidos aproximadamente de manera uniforme.
- **Peor caso:** O(n), si la distribución es muy desigual y cada estimación reduce poco el intervalo.
- **Espacio auxiliar:** O(1).

**Requisito:** claves numéricas ordenadas. Si los extremos tienen el mismo valor, la fórmula de interpolación debe manejar ese caso para evitar una división por cero.

### Casos de uso

- Buscar valores en conjuntos numéricos ordenados y aproximadamente uniformes, como ciertos rangos de identificadores o mediciones.
- Consultas en las que una buena estimación de la posición permite saltar rápidamente a la zona esperada.
- No es la opción adecuada cuando la distribución de los valores es desconocida o muy desigual; en ese caso, la búsqueda binaria ofrece un peor caso más predecible.

**Ejemplo:** en una lista ordenada de números repartidos uniformemente entre 0 y 1000, buscar 900 permite estimar una posición cercana al extremo superior, en lugar de comenzar por el centro.

---

## 5. Exponential Search (búsqueda exponencial)

### Definición

Encuentra primero un intervalo que pueda contener el objetivo. Comienza cerca del inicio y duplica el índice examinado (por ejemplo, 1, 2, 4, 8, 16) hasta superar el objetivo o llegar al límite. Después aplica búsqueda binaria dentro del intervalo encontrado.

### Eficiencia

- **Mejor caso:** O(1), si el objetivo está en una de las primeras posiciones revisadas.
- **Tiempo:** O(log i), donde i es la posición ordinal del objetivo contando desde 1; el peor caso para una colección de n elementos es O(log n).
- **Espacio auxiliar:** O(1) en la versión iterativa.

**Requisito:** secuencia ordenada y acceso eficiente a posiciones por índice. La búsqueda necesita poder consultar los índices que va duplicando.

### Casos de uso

- Buscar en un arreglo ordenado cuyo tamaño se desconoce o es muy grande.
- Buscar en una secuencia ordenada conceptualmente no acotada, cuando se puede acceder por índice.
- Consultas en las que se espera que el elemento esté cerca del comienzo; la fase de acotación evita recorrer previamente toda la colección.

**Ejemplo:** si el objetivo está alrededor de la posición 20, se prueba con índices 1, 2, 4, 8 y 16; al encontrar un límite superior en 32, se hace búsqueda binaria entre 16 y 32.

---

## 6. BST balanceado (árbol binario de búsqueda balanceado)

### Definición

Es un árbol binario que mantiene las claves en orden: las claves menores que un nodo están en su subárbol izquierdo y las mayores, en el derecho. Además, una regla de balance mantiene la altura del árbol proporcional a log n. Ejemplos conocidos son los árboles AVL y los árboles rojo-negro. Al insertar o eliminar elementos, puede ser necesario reorganizar nodos mediante rotaciones.

### Eficiencia

En un árbol balanceado:

- **Buscar:** O(log n) en el peor caso.
- **Insertar:** O(log n) en el peor caso, incluido el mantenimiento del balance.
- **Eliminar:** O(log n) en el peor caso, incluido el mantenimiento del balance.
- **Espacio:** O(n) para guardar los nodos y sus enlaces.

Si el árbol no se mantiene balanceado, puede degenerar en una cadena y estas operaciones pueden tardar O(n).

### Casos de uso

- Mantener una colección ordenada que recibe inserciones y eliminaciones frecuentes.
- Implementar conjuntos o mapas ordenados.
- Consultar mínimo, máximo, sucesor, predecesor o recorrer las claves en orden.
- Resolver consultas por rangos, como obtener todas las claves entre dos valores.

**Ejemplo:** un sistema que agrega y elimina registros por identificador y necesita listarlos en orden puede mantener esos identificadores en un árbol AVL o rojo-negro.

---

## 7. Hash Table (tabla hash)

### Definición

Es una estructura que almacena pares de clave y valor. Una función hash transforma cada clave en una posición o cubeta de una tabla. Si varias claves llegan a la misma posición, se produce una colisión; la estructura debe resolverla, por ejemplo, mediante encadenamiento o direccionamiento abierto.

### Eficiencia

- **Búsqueda, inserción y eliminación promedio esperadas:** O(1), si la función hash distribuye bien las claves y la tabla mantiene un factor de carga adecuado.
- **Peor caso:** O(n), si muchas claves colisionan y deben revisarse una por una.
- **Espacio:** O(n), con capacidad adicional para las cubetas y el crecimiento de la tabla.

El costo de calcular el hash también depende del tamaño de la clave. Una tabla hash no conserva el orden de las claves.

### Casos de uso

- Buscar rápidamente un valor por una clave exacta, como un usuario por su identificador.
- Implementar diccionarios, mapas, índices en memoria y conjuntos que necesitan comprobar pertenencia.
- Contar frecuencias o detectar elementos repetidos.

No es la mejor opción cuando se requieren resultados ordenados o consultas por intervalos, como “todas las claves entre 20 y 50”.

**Ejemplo:** en un directorio telefónico en memoria, la clave puede ser el nombre de usuario y el valor, su número telefónico; la tabla hash permite localizar el valor usando la clave.

---

## Conclusión

La elección depende de cómo están organizados los datos y de las operaciones que se necesitan. La búsqueda lineal funciona sin ordenamiento y es fácil de aplicar; la binaria es eficiente en arreglos ordenados; Jump Search, Interpolation Search y Exponential Search aprovechan propiedades específicas de los datos. Un BST balanceado conserva el orden y permite actualizar la colección, mientras que una Hash Table suele ofrecer búsquedas promedio muy rápidas por clave exacta, pero no mantiene orden.

## Fuentes de consulta

- [NIST: búsqueda lineal](https://www.nist.gov/dads/HTML/linearSearch.html)
- [NIST: búsqueda binaria](https://www.nist.gov/dads/HTML/binarySearch.html)
- [Universidad de Waterloo: Jump Search](https://ece.uwaterloo.ca/~dwharder/aads/Algorithms/Searching/Jump_search/)
- [Universidad de Waterloo: Interpolation Search](https://ece.uwaterloo.ca/~dwharder/aads/Algorithms/Searching/Interpolation_search/)
- [ETH Zürich: apuntes de búsqueda, incluida la búsqueda exponencial (PDF)](https://lec.inf.ethz.ch/DA/2017/slides/daAll.en.handout.4x4.pdf)
- [NIST: árbol AVL](https://www.nist.gov/dads/HTML/avltree.html)
- [OpenDSA, Virginia Tech: análisis de árboles de búsqueda binaria](https://opendsa-server.cs.vt.edu/ODSA/Books/wfu/csc201-b/fall-2026/201B/html/BST.html)
- [NIST: tabla hash](https://www.nist.gov/dads/HTML/hashtab.html)
`

