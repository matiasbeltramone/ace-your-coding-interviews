# Árboles (Trees)

Un árbol es una estructura de datos donde un nodo puede tener cero o más hijos. Cada nodo contiene un valor y, de forma similar a los grafos,
cada nodo puede tener una conexión entre otros nodos denominada link. **Un árbol es un tipo de grafo, pero no todos los grafos son árboles.**

- El nodo superior se llama `raíz`. El DOM, o Documento de modelo de objetos, es una estructura de datos de árbol.

- Un nodo sin hijos se llama nodo `hoja`.

- La altura del árbol es la distancia entre el nodo de la hoja más lejano y el nodo raíz.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111919184-696dcc00-8a67-11eb-8f2c-e674b4f58cb1.png"/></p>

## Árboles Binarios (Binary Trees)

Los **árboles binarios** son un tipo especial de árbol en el que cada nodo solo puede tener un **máximo de dos hijos**: un hijo izquierdo y un hijo derecho.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111919224-991cd400-8a67-11eb-8581-cd2589c1ff69.png"/></p>

### Tipos

#### Full Binary Trees

 - Un **full binary tree** es un árbol binario en el que cada nodo **tiene exactamente cero o dos hijos** (pero nunca uno).

#### Complete Binary Trees

 - Un **complete binary tree** es un árbol binario en el que todos los niveles excepto el último están llenos de nodos.

#### Perfect Binary Trees

 - Un **perfect binary tree** es un árbol binario en el que todos los niveles, incluido el último, están llenos de nodos.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111919302-16484900-8a68-11eb-895e-e0d717c8e87a.png"/></p>

#### Binary Search Trees

Un **binary search tree* es un tipo especial de árbol binario donde los valores de cada nodo a la izquierda son menores que el valor del nodo padre
y los valores de cada nodo a la derecha son mayores que su valor del nodo padre.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111929188-676e3200-8a94-11eb-8c81-450fd2ff5428.png"/></p>

## Coding A Binary Search Tree In JavaScript

Primero construyamos nuestra clase `Node`. Cada nodo puede tener como máximo dos hijos, por lo que los llamaremos `leftChild` y `rightChild`. Estos contendrán valores de Nodo.

```js
class Node {
  constructor(value) {
    this.value = value
    this.leftChild = null
    this.rightChild = null
  }
}
```

También crearemos una clase `BinaryTree`. El constructor no aceptará ningún argumento, pero tendrá un nodo raíz.

```js
class BinaryTree {
  constructor() {
    this.root = null
  }
}
```

## Adding Nodes

Creemos una función dentro de nuestra clase BinaryTree que agregará un nodo. Hay dos casos que debemos tener en cuenta:

- El arbol esta vacio
- Hay nodos en el árbol

### The Tree Is Empty:

Primero, si el árbol está vacío, establezca this.root igual a un nuevo Nodo que contenga este valor.

```js
addChild(value) {
  if (this.root === null) {
    this.root = new Node(value);
    return;
  }
}
```

### The Tree Has Nodes:

Si hay nodos en el árbol debemos atravesarlo hasta encontrar un lugar apropiado para agregarlo. Inicializaremos una variable, `currentNode` en el nodo raíz
(porque comenzamos en la raíz para recorrerlo) y una variable booleana, `added`, que se inicializará en `false` y solo se actualizará a `true` cuando hayamos agregado exitosamente el nodo.

Si bien no hemos agregado el nodo y todavía tenemos un nodo cuyo valor podemos comparar contra nuestro valor de `currentNode`,
queremos comparar el valor del nodo que se agregará con el valor del nodo actual.
Si el valor que queremos agregar es igual al valor del nodo actual, devuelveremos "No se pueden agregar duplicados".

Si el valor es menor que el valor del nodo actual, verifique si hay un espacio libre en el hijo izquierdo del nodo actual.
Si el valor es mayor que el valor del nodo actual, verifique si hay un espacio libre en el hijo derecho del nodo actual.
Si lo hay, ¡genial! Encontramos un lugar para agregar nuestro nodo y podemos actualizar nuestro booleano `added` para que sea `true`.
Si el lugar no está libre, debemos recorrer el subárbol izquierdo hasta encontrar un lugar que esté libre.

Podemos ver este proceso ilustrado en las siguientes imagenes:

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111929632-b23c7980-8a95-11eb-88ac-c53dde84bdf9.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111929634-b49ed380-8a95-11eb-8c36-27a51757581f.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111929651-bc5e7800-8a95-11eb-959e-de60a21bc2b8.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111929645-b9638780-8a95-11eb-98f1-c9a3a4a03d0c.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111929641-b799c400-8a95-11eb-9584-b59f8492a068.png"/></p>

#### En Javascript lo podemos representar de la siguiente manera:

```js
addChild(value) {
  if (this.root === null) {
    this.root = new Node(value);
    return;
  } else {
    let currentNode = this.root;
    let added = false;
    
    while (!added && currentNode) {
      // Don't add duplicates
      if (value === currentNode.value) {
        return "Duplicates cannot be added";
      }
    
      if (value < currentNode.value) {
        // If the spot is free, add it
        if (currentNode.leftChild === null) {
          currentNode.leftChild = new Node(value);
          added = true;
        } else {
          // Otherwise find the next free spot
          currentNode = currentNode.leftChild;
        }
      } else if (value > currentNode.value) {
          if (currentNode.rightChild === null) {
            currentNode.rightChild = new Node(value);
            added = true;
          } else {
            currentNode = currentNode.rightChild;
          }
      }
    }
  }
}
```
