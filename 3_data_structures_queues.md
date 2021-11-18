# Colas

Las colas son muy similares a las pilas, sin embargo, utilizan el paradigma de "primero en entrar, primero en salir" (FIFO).
Esto significa que el elemento más antiguo (el elemento que se agregó primero) es el siguiente elemento que se eliminará de la cola.
Podemos imaginar una cola como una cola de personas esperando para comprar entradas para el cine.
La persona que ha estado esperando en la fila por más tiempo es la siguiente persona a la que se le dará servicio.

<p align="center"><img width="50%" src="https://user-images.githubusercontent.com/22304957/111905542-f5adce00-8a2a-11eb-83cb-faabbe99a0f3.png"></p>

## Casos de Uso

Las colas son similares a las listas enlazadas (que veremos luego en otro post) y se utilizan normalmente en búsquedas de árboles que dan prioridad a la amplitud
(que también trataremos en un post posterior). También podes ver que se utilizan colas para implementar caché.

## Desventajas

Las colas son mucho más difíciles de actualizar al agregar y quitar elementos que una pila porque agregamos elementos a un lado de la estructura
y los quitamos del otro lado.

## Métodos

Las colas utilizan tres métodos principales (enqueue, dequeue, peek) y algunos métodos de ayuda (isEmpty, get lenght).

- enqueue() : Add an item to the back of the queue
- dequeue() : Remove an item from the front of the queue
- peek() : Return the item at the front of the queue (but do not remove it)
- isEmpty() : Check whether the queue is empty
- get length() : Return the length of the queue

## Implementación en Javascript

Estaré codificando esta cola usando la notación de clases de JavaScript al igual que en las pilas, pero también puedes crear una cola con funciones.
Lo primero que haremos es crear una clase `Queue` y darle un constructor con una propiedad: `queue`. Construiremos esta cola usando una matriz.
El frente (front, es decir, el primer elemento de la matriz) de la cola será el frente de la matriz y la parte posterior (back) de la cola, donde
agregamos nuevos elementos, será el final de la matriz.

```javascript
export default class Queue {
  constructor() {
    this.queue = [];
  }
}
```

Primero, creemos una propiedad de longitud que devolverá la longitud de la cola. Usaremos una función getter para poder acceder a la longitud con `queue.length`.

```javascript
get length() {
  return this.queue.length;
}
```

Ahora escribamos el método de agregar a la cola que tomará un elemento y lo agregará a nuestra cola.
Recordemos que estamos agregando elementos al final de la matriz por lo que podemos usar el método nativo `array.push()`.

```javascript
enqueue(item) {
  this.queue.push(item);
}
```

El método de eliminación de la cola eliminará el elemento al principio de la cola. Dado que el frente de nuestra cola es el comienzo de la matriz,
podemos usar el método `array.shift()`.

```javascript
dequeue() {
  return this.queue.shift();
}
```

Para verificar qué elemento está al frente de nuestra cola, podemos crear un método de inspección `peek`.
El elemento al principio de la cola es el primer elemento de la matriz, por lo que podemos acceder a él con matriz[0].

```javascript
peek() {
  return this.queue[0];
}
```

Finalmente, agreguemos un método auxiliar, `isEmpty`, que devuelve un valor booleano que indica si la cola tiene elementos o no.

```javascript
isEmpty() {
  return this.length === 0;
}
```

### Resultado:

```javascript
export default class Queue {
  constructor() {
    this.queue = [];
  }
  
  get length() {
    return this.queue.length;
  }
  
  enqueue(item) {
    this.queue.push(item);
  }
  
  dequeue() {
    return this.queue.shift();
  }
  
  peek() {
    return this.queue[0];
  }
  
  isEmpty() {
    return this.length === 0;
  }
}
```
