# Stacks (Pilas)

Una pila es una estructura de datos del tipo "último en entrar, primero en salir" (suelen llamarse estructuras LIFO), lo que significa que el elemento más nuevo (o el elemento que se agregó en último lugar)
en la pila será el primero que se elimine de la estructura. Podemos pensar en una pila como una pila de libros. Para llegar al tercer libro de la pila,
tenemos que quitar el quinto libro y luego el cuarto libro, algunos se pasarán de listos y pensarán simplemente los levantan todos a la vez y me salteo pasos, pero síganme en este ejemplo.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/111905554-fcd4dc00-8a2a-11eb-8d25-6899a64fe225.png"/></p>

## Ventajas de las Pilas

Las pilas permiten agregar y quitar en tiempo constante el elemento superior de la estructura. El tiempo constante, llamado **O(1)**, es un tiempo de ejecución muy eficaz
(ya hablamos de la notación Big-O y el tiempo de ejecución en otro post podes verlo por acá: <a>https://matiasbeltramone.github.io/algorithmic-complexity/</a>).
Estas acciones son de tiempo constante porque no necesitamos mover ningún elemento para sacar el elemento superior de la pila.

## Desventajas de las Pilas

Las pilas, desafortunadamente, no ofrecen acceso en tiempo constante al enésimo elemento de la pila, a diferencia de una matriz (array).
Entonces, si queremos acceder al tercer libro, tendríamos que sacar cada elemento de nuestra pila hasta llegar al tercer libro, y si queremos acceder al primer elemento
de la pila (el elemento inferior) tenemos que recorrer cada libro que se encuentra encima y sacarlo; esto tiene un tiempo de ejecución en el peor de los casos de **O(n)**
donde n es el número de libros en la pila.

Por el contrario, podemos acceder a índices específicos en matrices con notación entre corchetes. Si queremos el tercer elemento de una matriz, podemos acceder
a él en tiempo constante con la forma: `matriz[2]`, si alguien se pregunta porque usamos [2] simplemente porque en programación el primer elemento de una matríz es [0].

## Métodos de una Pila

Hay tres métodos principales **(push, pop, peek)** en una pila y algunos métodos adicionales (isEmpty, getLength) para ayudar con diferentes tareas:

- pop() : Removes the top item from the stack
- push(item) : Adds an item to the top of the stack
- peek() : Returns the item at the top of the stack (**but does not remove it**)
- isEmpty() : Returns true if the stack is empty
- getLength() : Returns the number of items in the stack

## Codificando una Pila en JavaScript

Estaré codificando esta pila usando la notación de clase de JavaScript ya que estoy acostumbrado de esta manera, pero también puedes crear una pila con funciones.
Lo primero que haremos es crear una **clase Stack** y darle un constructor con una propiedad: stack. Construiremos esta pila usando una matriz (array).
La parte superior de la pila será el final de la matriz [n] y la parte inferior de la pila será el comienzo de la matriz [0].
Estoy estructurando mi pila de esta manera para poder usar los métodos nativos de Javascript `array.push()` y `array.pop()`.
Si quisiéramos agregar y eliminar elementos desde el principio de la matriz en lugar del final, tendríamos que usar `array.unshift()` y `array.shift()`.

```javascript
class Stack {
  constructor() {
    this.stack = [];
  }
}
```

Primero, creemos nuestra función de obtención de longitud. Esto mantendrá un registro de la cantidad de elementos que posee nuestra pila.
Estoy usando una función getter para poder acceder a la longitud de la pila con `stack.length` en lugar de crear una función `getLength()` que se debe llamar cada vez
que quiero verificar la longitud.

```javascript
get length() {
  return this.stack.length;
}
```

Ahora creemos el método push que agrega un elemento a la parte superior de la pila. Podemos usar el método `array.push()`
(ya que la parte superior de nuestra pila es el final de la matriz). Este método toma un argumento: el elemento a agregar.

```javascript
push(item) {
  return this.stack.push(item);
}
```

A continuación, creemos el método pop que elimina el elemento superior de la pila y lo devuelve.
Dado que hemos establecido que la parte superior de la pila es el final de nuestra matriz, podemos usar el método `array.pop()`.

```javascript
pop() {
  return this.stack.pop();
}
```

Para ver qué elemento se encuentra en la parte superior de la pila, podemos crear un método de inspección (peek).
Solo tenemos que verificar qué elemento vive en el último índice de nuestra pila.

```javascript
peek() {
  return this.stack[this.length - 1];
}
```

Aclaración: `[this.length - 1]` es el último elemento ya que el primer elemento de la matríz se encuentra en la posición 0.

También es posible que deseemos tener una función auxiliar para verificar si nuestra pila está vacía.
Por supuesto, puede omitir este método y simplemente verificar this.length === 0, sin embargo,
personalmente me gusta tener esta función auxiliar ya que es mas semántica a la hora de escribir código.

```javascript
isEmpty() {
  return this.length === 0;
}
```

### Pila en Javascript:

```javascript
class Stack {
  constructor() {
    this.stack = [];
  }
  
  get length() {
    return this.stack.length;
  }
  
  push(item) {
    return this.stack.push(item);
  }
  
  pop() {
    return this.stack.pop();
  }
  
  peek() {
    return this.stack[this.length - 1];
  }
  
  isEmpty() {
    return this.length === 0;
  }
}
```

## ¿Cuando usarías una Pila durante una entrevista?

Intentemos pensar en un caso de uso en el que una pila pueda resultar útil durante una entrevista técnica. Tómate unos minutos y pensá si podes idear un escenario.
Tene en cuenta que las pilas son una estructura de datos tipo: **último elemento en entrar, primero en salir,** lo que significa que el elemento agregado más recientemente
será el primero en eliminarse cuando se llame a la función `pop()`.

¿Pudiste pensar en una pregunta de entrevista técnica en la que una pila sería una buena estructura de datos para usar?

Si es así, ¡genial! Si tuviste problemas para pensar en un escenario, ¡no te preocupes! Cuanto más aprendas y practiques el uso de estructuras de datos,
más rápidamente podrás reconocer las preguntas que requieren ciertas estructuras de datos.

Aquí hay una pregunta de ejemplo en la que una pila sería una estructura de datos óptima:

Imaginate que estás creando un componente de navegación para el sitio web de un producto que llamaremos "Freelancers". Los usuarios pueden navegar desde la página de inicio
a diferentes páginas de productos dentro del sitio.

Por ejemplo, un flujo de usuarios podría ser `Home > Freelancers > Backend > Java`.

### Actividad

Crea un componente de navegación que muestre la lista de páginas visitadas anteriormente, así como un botón de retroceso que permita al usuario volver a la página anterior.

Una pila sería una solución óptima para esta pregunta porque el elemento más alto en una pila es el elemento agregado más recientemente
(en este caso, la última página que se visitó). Entonces, cuando queremos navegar hacia atrás, podemos sacar el último elemento de la pila y representar el estado
de la página anterior.

Su solución debe mostrar una navegación con cuatro enlaces, una lista del historial que muestra las páginas visitadas anteriormente,
un párrafo de la página actual que muestra el nombre de la página que se muestra actualmente y un botón de retroceso que, cuando se hace click,
navega a la página visitada anteriormente.

Tómese su tiempo para desarrollar su solución. Hable en voz alta sobre su solución para practicar la solución tradicional de estilo de pizarra.
Incluso si confía en sus habilidades de comunicación, es importante que practique expresar sus pensamientos.

¿Ha desarrollado una solución? Si es así, ¿ha probado varios flujos de usuarios? ¿Se rompe algo? Si es así, ¡arréglalo!

Aquí hay algunos errores adicionales en los que pensar:

1. ¿Qué sucede si no tiene ninguna página visitada anteriormente?
2. ¿Qué sucede si ya estás en una página e intentas navegar hasta ella?

Ahora que ha tenido en cuenta algunos casos de uso especiales, ¿hay alguna mejora que pueda realizar para optimizar el rendimiento o la experiencia del usuario?

## Follow-Up Question

A menudo, durante los desafíos de codificación, se te planteará una pregunta inicial y, si terminas una parte, el entrevistador te hará algunas preguntas de seguimiento.
Aquí hay un par de ejemplos que puede encontrar para esta pregunta. A menudo, no se espera que complete todas las partes de la pregunta o que las haga completamente
correctas: estos desafíos son más para descubrir cómo resuelve y comunica los problemas planteados.

- **Amplíe su solución para poder navegar hacia adelante y hacia atrás.**
