# Listas enlazadas (Linked List)

Las listas enlazadas son una serie de nodos vinculados donde cada nodo apunta al siguiente nodo de la lista.
Cada nodo tiene un valor y un puntero al siguiente nodo. También hay listas doblemente enlazadas en las que cada nodo también apunta al nodo anterior de la lista.

Las listas enlazadas usan el método "último en entrar, primero en salir" (similar a una pila, FIFO) donde los nodos se agregan y eliminan del mismo extremo.

Para buscar un nodo en una lista enlazada, tenemos que comenzar en el nodo principal (el primer nodo de la lista) e iterar a través de cada nodo hasta encontrarlo
o llegar al final de la lista. En el peor de los casos, esto significa que la búsqueda de un elemento tiene un tiempo de ejecución de **O(n)** donde n es el
número de elementos de la lista.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111906025-67871700-8a2d-11eb-877e-2df11388419d.png"/></p>

## Linked List Methods

Las listas enlazadas usan dos métodos primarios `push & pop` además de algunos métodos auxiliares `get index, delete, isEmpty`.

- push(Node) : Add an element to the linked list
- pop() : Remove an element from the linked list
- get(index) : Return an element from a given index (but don't remove it)
- delete(index) : Delete an item from a given index
- isEmpty() : Return a boolean indicating whether the list is empty

## Singly-Linked Lists vs. Doubly-Linked Lists

Las listas enlazadas pueden ser simples o doblemente enlazadas. En una lista enlazada simple, cada nodo tiene **un puntero** que apunta al siguiente elemento de la
lista. En una lista doblemente enlazada, cada nodo tiene **dos punteros**: uno que apunta al siguiente elemento de la lista y otro que apunta al elemento anterior
de la lista.

Las listas doblemente enlazadas son excelentes para eliminar nodos porque brindan acceso a los nodos anterior y siguiente.
Para eliminar un nodo de una lista enlazada simple, tenemos que iterar a través de la lista, haciendo un seguimiento del nodo anterior
para que sea un poco más complicado.

Para nuestros propósitos de hoy, codificaremos una lista enlazada simple, pero tenga en cuenta que existen listas doblemente enlazadas.

## Implementación de Lista Enlazada

Primero construyamos nuestra clase `Node`. Los nodos tienen un valor y un puntero al siguiente nodo (para listas enlazadas simples, que es lo que crearemos).
Cuando creamos un nuevo nodo pasaremos el valor al constructor. También inicializaremos el puntero a nulo (ya que estamos agregando este nodo al final de la lista).

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
```

Ahora podemos crear nuestra clase `LinkedList`. El constructor realizará un seguimiento de tres cosas:

- head : The head pointer that keeps track of the first node in the linked list
- tail : The tail pointer that keeps track of the last node in the linked list
- length : The number of nodes in the list

Los punteros de cabeza y cola serán nulos hasta que agreguemos nuestro primer nodo.

```javascript
class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
    this.length = 0;
  }
}
```

Primero creemos nuestro método isEmpty que devuelve verdadero si no hay nodos en la lista.

```javascript
isEmpty() {
  return this.length === 0;
}
```

## Agregando Nodos

Creemos nuestro método `push` que toma un valor y agrega un nodo al final de la lista enlazada.
¡Recordemos que tenemos que actualizar el puntero de la cola cada vez que agregamos un nuevo nodo!

Analicemos dos escenarios que cambian la forma en que agregamos nodos.

### Lista Vacia

Si la lista está vacía, podemos establecer punteros de cabeza y cola al nodo recién agregado y luego incrementar la longitud.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111906821-47f1ed80-8a31-11eb-84b4-9af9a7efc8b6.png"/></p>

```javascript
push(value) {
  const newNode = new Node(value);
  
  if (this.isEmpty()) {
    this.head = newNode;
    this.tail = newNode;
  }
}
```

### Lista con Nodos

Si la lista no está vacía, primero debemos establecer el puntero del nodo de cola actual al nuevo nodo.
Luego podemos establecer `this.tail` en el nuevo nodo e incrementar la longitud de la lista.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111907113-ad92a980-8a32-11eb-989c-bf63a4681a4b.png"/></p>

```javascript
push(value) {
  const newNode = new Node(value);
  
  if (this.isEmpty()) {
    this.head = newNode;
    this.tail = newNode;
  } else {
    this.tail.next = newNode;
    this.tail = newNode;
  }
  
  this.length++;
}
```

## Eliminando Nodos

Ahora escribamos nuestro método `pop` que elimina un nodo en la cola. Tenemos un par de escenarios a considerar para eliminar un nodo.

### Lista Vacia

Si la lista está vacía, regresemos nulo

```javascript
pop() {
  if (this.isEmpty()) {
    return null
  }
}
```

### Un Solo Nodo

Si solo hay un nodo en la lista, tenemos que restablecer los punteros de cabeza y cola a nulos y disminuir la longitud.
¿Cómo sabemos si nuestra lista tiene un elemento? Podemos comprobar la propiedad de longitud o comprobar si los punteros de cabeza y cola
se refieren al mismo elemento.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111907187-f185ae80-8a32-11eb-853a-ccd63e189cd0.png"/></p>

```javascript
pop() {
  /* List is empty */
  if (this.isEmpty()) {
    return null;
  } else if (this.length === 1) {
    /* There is one node in the list */
    const nodeToRemove = this.head;
    
    this.head = null;
    this.tail = null;
    this.length--;
    
    return nodeToRemove;
  }
}
```

### Más de un Nodo

Si hay varios nodos en la lista tenemos que seguir algunos pasos. Recordemos que no podemos simplemente acceder a un nodo específico en una lista enlazada
y en una lista enlazada simple solo tenemos acceso al siguiente elemento de la lista. Por lo tanto, para sacar el elemento final de la lista,
primero debemos encontrar el nodo que apunta al nodo final.

Una vez que encontramos el penúltimo nodo, tenemos que asignarlo a una variable para que podamos devolverlo.
Llamaremos a esta variable `nodeToRemove` y la configuraremos en el nodo de cola actual. Luego, actualizaremos el puntero tail para que apunte
al penúltimo nodo y, finalmente, actualizaremos el nuevo nodo tail para que apunte a nulo.

A continuación, se muestra un resumen de estos pasos. Puede ver este proceso ilustrado luego.

```javascript
/*
  While there are nodes in the list
    If the next node in the list is the tail
      Update the tail pointer to point to the current node
      Set the new tail node to point to null
      Decrement the length of the list
      
      Return the previous tail element as removed
*/
```

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111907855-d5cfd780-8a35-11eb-9fbf-3b5440c02c21.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111907888-e6804d80-8a35-11eb-819a-f02b5bfe88ee.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111907912-f9931d80-8a35-11eb-9f1b-7e369ba62d65.png"/></p>

```javascript
pop() {
  /* List is empty */
  if (this.isEmpty()) {
    return null;
  } else if (this.length === 1) {
    /* There is one node in the list */
    const nodeToRemove = this.head;
    this.head = null;
    this.tail = null;
    this.length--;
    
    return nodeToRemove;
  } else {
    /* There are multiple nodes in the list */
    // Start our pointer at the head
    let currentNode = this.head;
   
    // We're removing the last node in the list
    const nodeToRemove = this.tail;
    // This will be our new tail
    let secondToLastNode;
    
    while (currentNode) {
      if (currentNode.next === this.tail) {
        secondToLastNode = currentNode;
        break;
      }
      
      currentNode = currentNode.next;
    }
    
    secondToLastNode.next = null;
    this.tail = secondToLastNode;
    this.length--;

    return nodeToRemove;
  }
}
```

## Obtener Nodos

Creemos ahora un método `get` que toma un índice y devuelve el nodo que se encuentra en ese índice.

Necesitamos verificar tres casos:

- El índice solicitado está fuera de los límites de la lista.
- La lista está vacía
- Solicitamos el primer o el último elemento.

### El índice no es válido:

Si el índice solicitado está fuera de los límites de la lista, o si la lista está vacía, devolveremos un valor nulo.

```javascript
get(index) {
  if (index < 0 || index > this.length || this.isEmpty()) {
    return null;
  }
}
```

### Primer o último Nodo

Si estamos solicitando el primer índice, devolvemos el nodo `head`. Si estamos solicitando el último índice `(this.length - 1)`, devolvemos el nodo `tail`.

```javascript
get(index) {
  if (index < 0 || index > this.length || this.isEmpty()) {
    return null;
  }
  
  /* We want the first node */
  if (index === 0) {
    return this.head;
  }
  
  /* We want the last node */
  if (index === this.length - 1) {
    return this.tail;
  }
}
```

### Nodo en el medio

Si el índice es un índice válido y no es el primer o el último nodo de la lista, debemos recorrer la lista en iteración hasta encontrar el índice pedido.
Podemos realizar un seguimiento del índice actual con una variable de iterador.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111908311-8094c580-8a37-11eb-9ded-1864b4ad77a7.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111910679-2862c100-8a41-11eb-843d-a2f1c90b087c.png"/></p>

```javascript
  get(index) {
    if (index < 0 || index > this.length || this.isEmpty()) {
      return null;
    }
    
    /* We want the first node */
    if (index === 0) {
      return this.head;
    }
    
    /* We want the last node */
    if (index === this.length - 1) {
      return this.tail;
    }
    
    /* We want a node somewhere in the list */
    let currentNode = this.head;
    let iterator = 0;
    
    while (iterator < index) {
      iterator++;
      currentNode = currentNode.next;
    }
    
    return currentNode;
}
```
<hr>
<p align="center">Copyright © Todos los derechos reservados a Emma Bostian</p>
<p align="center">Adaptación realizada para la comunidad hispana</p>
