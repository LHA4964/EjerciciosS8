# Ejerecicios Semana 8

## Autor

Jesus Alejandro Hernandez Mendez y Luis Holgado Arranz

## Respuestas:

### 105. Explique qué es una función hash.

Una función hash es una función que transforma datos de entrada (como un objeto o una cadena de texto) en un valor numérico entero, llamado código hash. Este valor se usa comúnmente para indexar datos rápidamente en estructuras como tablas hash.

### 106. Explique qué función hash utiliza Java para objetos de la clase Integer.

En Java, la clase Integer usa una función hash trivial: simplemente devuelve el valor entero en sí.
Ejemplo: public int hashCode() {
    return intValue();
}

### 107. Explique qué función hash utiliza Java para objetos de la clase String.

Java implementa la función hash de String con la siguiente fórmula:
s[0]*31ⁿ⁻¹ + s[1]*31ⁿ⁻² + ... + s[n-1]
Donde s[i] es el carácter en la posición i y n es la longitud de la cadena. Esto se traduce en:
public int hashCode() {
    int h = 0;
    for (int i = 0; i < value.length; i++) {
        h = 31 * h + value[i];
    }
    return h;
}

### 108. Explique cómo puede implementarse un map mediante hashing.

Un Map puede implementarse con una tabla hash en la que:

-Cada clave (key) se convierte a un índice mediante una función hash.
-Los valores (value) se almacenan en el índice correspondiente.
-Si ocurre una colisión (dos claves tienen el mismo índice), se maneja con:
-Encadenamiento (chaining): listas enlazadas en cada celda.
-Dirección abierta (open addressing): buscar otra celda disponible.

En Java, HashMap funciona así.

### 109. Explique cómo puede implementarse un conjunto mediante hashing.

Un Set almacena valores únicos sin claves. Puede implementarse igual que un Map, usando los valores como claves:
-Se usa una tabla hash donde cada valor tiene su hash.
-Si el valor ya existe (misma posición e igual por equals), no se añade.
-Internamente, HashSet usa un HashMap donde el valor del Set es la clave del Map, y el valor asociado es un marcador interno (como un objeto constante).

### 110. Explique por qué Java aumenta el tamaño máximo de una tabla hash antes de que se llene.

Java expande la tabla hash (rehashing) cuando alcanza un cierto porcentaje de ocupación, llamado factor de carga (por defecto, 0.75):
-Para evitar muchas colisiones, que ralentizan la búsqueda, inserción y eliminación.
-Aumentar el tamaño mantiene un buen rendimiento (casi O(1)).

### 111. Explique por qué Java recoloca los elementos de una tabla hash cuando ésta aumenta de tamaño.

Cuando la tabla se redimensiona, todos los elementos deben ser rehashados, porque:
-La función hashCode() da el mismo valor, pero el índice dentro de la tabla depende de su tamaño (index = hash % capacity).
-Al cambiar la capacidad, los índices cambian, por eso es necesario recolocarlos para que estén en la posición correcta.

### 112. Explique por qué es conveniente especificar el tamaño inicial de una HashSet o de un HashMap cuando se espera que éste sea grande.

Porque así evitas múltiples redimensiones a medida que crece:
-Cada redimensionamiento implica rehash y reubicación de todos los elementos, lo cual es costoso en tiempo y memoria.
-Si sabes que vas a almacenar, por ejemplo, 10,000 elementos, establecer un tamaño inicial adecuado mejora el rendimiento.

### 113. ¿Qué consecuencias tiene el no reescribir el método hashCode
cuando se define un tipo de elemento para un HashSet o HashMap?

Si no sobreescribes hashCode (y equals) correctamente:
-Objetos iguales lógicamente podrían tener hashes diferentes, causando:
-Que se almacenen múltiples veces en un Set.
-Que no se puedan recuperar correctamente de un Map.

### 114. Ponga algún ejemplo en que sea conveniente utilizar una cola con prioridad.

Sala de urgencias: no se atienden por orden de llegada, sino por nivel de gravedad.
- Un paciente con fiebre (prioridad baja)
- Un paciente con un infarto (prioridad alta)

Control de tráfico aéreo: los aviones solicitán permiso y se les asigna turno
- Un avión sin combustible (prioridad alta)
- Un avión con mercancía (prioridad baja)
- Un avión comercial (prioridad media)

### 115. Muestre el proceso de inserción de los valores: 5, 7, 8, 1, 4, 3 en una cola con prioridad implementada mediante un heap. A continuación, muestre el proceso de sacar el primer elemento de la cola

Al insertar los valores en una cola con prioridad implementada mediante un heap mínimo, se va manteniendo la propiedad de que cada nodo padre es menor que sus hijos. 

Primero se inserta 5, luego 7 y 8, que se colocan como hijos sin necesidad de reordenar. 
Al insertar 1, este sube hasta la raíz al ser el menor. Luego se inserta 4, que sube por encima de 5, y después 3, que sube por encima de 8. 
El heap resultante es: [1, 4, 3, 7, 5, 8]. 

Al eliminar el primer elemento (1), se coloca el último valor (8) en la raíz y se reordena haciendo bajadas sucesivas: 
primero se intercambia con 3, y luego con 5, quedando el heap ajustado como [3, 4, 5, 7, 8]. 

### 116. Se ha implementado una cola con prioridad mediante un heap. En un momento dado, su contenido es el que se muestra en la figura 1. Se pide mostrar el proceso de sacar el primer elemento de la cola

La cola con prioridad implementada con un heap contiene inicialmente los elementos:
[11, 12, 14, 17, 19, 17, 46, 43, 23, 28, 29].


Al eliminar el primer elemento (11), que está en la raíz del heap, se reemplaza por el último valor (29). 
Ahora el heap comienza con 29, y se debe restaurar la propiedad del heap mínimo. 
Se compara 29 con sus hijos (12 y 14) y se intercambia con el menor, que es 12. Luego se compara con los hijos de la nueva posición (17 y 19), y se intercambia con 17. Después, se compara con los hijos de su nueva posición (43 y 23), y se intercambia con 23. 

El heap final después de sacar el primer elemento queda:
[12, 17, 14, 23, 19, 17, 46, 43, 29, 28].

### 117. Realice el análisis de complejidad temporal asintótica de las operaciones de inserción y de sacar primero de la cola en la implementación con un heap de una cola con prioridad

- Complejidad temporal de inserción:

Al agregar un nuevo elemento en el heap, lo que hacemos es agregar el elemento al final del árbol (última posición en el arreglo) y luego "subimos" ese elemento para restaurar la prioridad del heap.

La cantidad de comparaciones e intercambios que se realizan depende de la altura del heap. Un heap es un árbol binario completo, por lo que su altura es O(log n), donde n es el número de elementos en el heap.

- Complejidad temporal de sacar primero:

Para eliminar el elemento con mayor prioridad (que siempre estará en la raíz del heap), tomamos el valor en la raíz, lo eliminamos y luego reordenamos el heap para restaurar su propiedad.

La cantidad de comparaciones y movimientos depende de la altura del heap, que es O(log n), donde n es el número de elementos en el heap.

### 118. ¿Qué clase de Java implementa colas con prioridad con heap?

Implementa colas con prioridad la clase PriorityQueue, está basada en una estructura de heap binario. Por defecto, implementa un min-heap (el elemento con la menor prioridad siempre estará en la raíz, es decir, el primer elemento de la cola). 

Los elementos se ordenan según el orden natural de los objetos (si implementan Comparable), o bien según un comparador personalizado si se proporciona uno al crear la cola.
