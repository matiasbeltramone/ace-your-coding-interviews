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

## Removing A Node

Eliminar un nodo es un poco más complicado, por lo que si no lo entendemos al principio, no hay que frustrarse. Veamos las ilustraciones para ver una representación visual de la teoría que vamos a estar viendo.

Al eliminar un nodo, hay cuatro casos a tener en cuenta:

- No se encuentra el nodo
- El nodo es un nodo hoja (sin hijos)
- El nodo tiene un hijo
- El nodo tiene dos hijos

Para eliminar un nodo, debemos realizar un seguimiento del nodo padre. Si se encuentra el nodo, redirigiremos al hijo izquierdo o derecho del padre (según el nodo que queramos eliminar) a uno de los hijos (si hay hijos) que empalmará el nodo.

También tendremos un booleano, encontrado (found), que hará un seguimiento de si hemos encontrado o no el nodo que queremos eliminar.

Si no se encuentra el nodo, devolveremos "No se encontró el nodo".

```js
removeChild(value) {
  let currentNode = this.root;
  let found = false;
  let nodeToRemove;
  let parentNode = null;
}
```

### Pasos a seguir:

Debemos recorrer el árbol buscando el nodo que queremos eliminar.

Este es un proceso similar a agregar un nodo (excepto que no estamos buscando un lugar libre, estamos buscando un valor exacto), pero el proceso de búsqueda de los subárboles izquierdo y derecho hasta que se encuentra el valor sigue siendo el mismo.

Solo recuerde que antes de atravesar el subárbol izquierdo o derecho, tenemos que establecer `parentNode` igual a `currentNode` para tener un control en el padre cuando llegue el momento de eliminación.

```js
removeChild(value) {
  let currentNode = this.root;
  let found = false;
  let nodeToRemove;
  let parentNode = null;
  
  // Find the node we want to remove
  while (!found) {
    if (currentNode === null || currentNode.value === null) {
      return "The node was not found";
    }
    
    if (value === currentNode.value) {
      nodeToRemove = currentNode;
      found = true;
    } else if (value < currentNode.value) {
      parentNode = currentNode;
      currentNode = currentNode.leftChild;
    } else {
      parentNode = currentNode;
      currentNode = currentNode.rightChild;
    }
  }
}
```

Una vez que hayamos encontrado el nodo, tenemos que dar cuenta de los últimos tres casos (nodo hoja, un hijo y dos hijos).

### The Node Is A Leaf Node:

Si el nodo que queremos eliminar es un nodo hoja (no tiene hijos), podemos establecer el hijo izquierdo o derecho de `parentNode` en nulo.

Creemos una variable auxiliar adicional para legibilidad llamada `nodeToRemoveIsParentsLeftChild` que devuelve verdadero si el nodo que estamos eliminando es el hijo izquierdo y falso si el nodo que estamos eliminando es el hijo derecho.

```
  const nodeToRemoveIsParentsLeftChild = parentNode.leftChild === nodeToRemove
  
  // If nodeToRemove is a leaf node, remove it
  if (nodeToRemove.leftChild === null && nodeToRemove.rightChild === null) {
    if (nodeToRemoveIsParentsLeftChild) {
      parentNode.leftChild = null
    } else {
      parentNode.rightChild = null
    }
  }
```

### The Node Has One Child:

Si el nodo que estamos eliminando tiene un hijo, podemos redirigir el puntero del nodo principal al hijo.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111930411-a2be3000-8a97-11eb-97fc-420e3a99ac63.png"/></p>

```js
else if (nodeToRemove.leftChild !== null && nodeToRemove.rightChild === null) {
  // Only has a left child
  if (nodeToRemoveIsParentsLeftChild) {
    parentNode.leftChild = nodeToRemove.leftChild;
  } else {
    parentNode.rightChild = nodeToRemove.leftChild;
  }
} else if (nodeToRemove.rightChild !== null && nodeToRemove.leftChild === null) {
  // Only has a right child
  if (nodeToRemoveIsParentsLeftChild) {
    parentNode.leftChild = nodeToRemove.rightChild;
  } else {
    parentNode.rightChild = nodeToRemove.rightChild;
  }
}
```

### The Node Has Two Children:

Si el nodo que estamos eliminando tiene dos hijos, se vuelve un poco más complicado. Aquí está el algoritmo:

- Establezca el puntero de `parentNode` en el subárbol derecho del nodo que estamos eliminando.
   - Si el hijo derecho del `parentNode` apunta al nodo que estamos eliminando, establezca `parentNode.rightChild` en `nodeToDelete.rightChild`.
   - Si el hijo izquierdo del `parentNode` apunta al nodo que estamos eliminando, establezca `parentNode.leftChild` en `nodeToDelete.rightChild`.

- Atraviesa las ramas izquierdas del subárbol derecho hasta encontrar un lugar libre.

- Una vez que se haya encontrado un lugar libre en el subárbol izquierdo, agregue el subárbol izquierdo de `nodeToDelete`.

Si eso es confuso, no se preocupe; también me confundió. Ahora mismo veremos este concepto en forma ilustrada.

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111930674-3a238300-8a98-11eb-992c-bbf1ff413788.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111930685-3d1e7380-8a98-11eb-9213-327718f57ee0.png"/></p>

<p align="center"><img width="70%" src="https://user-images.githubusercontent.com/22304957/111930688-40b1fa80-8a98-11eb-808b-1aec9f9ff3f2.png"/></p>

```js
  else {
    // Has two children
    const rightSubTree = nodeToRemove.rightChild;
    const leftSubTree = nodeToRemove.leftChild;

    // Set parent node's respective child to the right sub tree
    if (nodeToRemoveIsParentsLeftChild) {
      parentNode.leftChild = rightSubTree;
    } else {
      parentNode.rightChild = rightSubTree;
    }

    // Find the lowest free space on the left side of the
    // right sub tree and add the leftSubTree
    let currLeftNode = rightSubTree;
    let currLeftParent;
    let foundSpace = false;

    while (!foundSpace) {
      if (currLeftNode === null) {
        foundSpace = true;
      } else {
        currLeftParent = currLeftNode;
        currLeftNode = currLeftNode.leftChild;
      }
    }

    currLeftParent.leftChild = leftSubTree;

    return "The node was successfully deleted";
  }
}
```

<p align="center">Copyright © Todos los derechos reservados a Emma Bostian</p>
<p align="center">Adaptación realizada al Español</p>
