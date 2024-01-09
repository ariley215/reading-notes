# Data Structures and Algorithms

Big O- an equation that describes the runtime scales with input variables
 
- notation used to describe how efficient an algorithm is

1. If you have 2 different steps they get added together
2. Drop constants
3. Different inputs get different variables
4. Drop non dominate terms

Linked List- Elements in a linked list are known as nodes
Node - contains a value and a pointer value 
 Each node contains a key and a pointer to its successor node, known as next 
 frst node: head || last node: tail

- good at add and delete nodes, and searching
- not good at searching and retrieving

![Linked List](https://miro.medium.com/v2/resize:fit:720/format:webp/1*4fuF6lHXOSmoVNcOV8aaJA.png)

Arrays- continous block of cells

- Good a retrieving items, updating, and searching
- Not good at adding/ deleting
- Used for different sorting algorithms such as insertion sort, quick sort, bubble sort and merge sort.

![Array](https://miro.medium.com/v2/resize:fit:720/format:webp/1*pYIKtQYbX8vgCWrwe1YOyg.png)

Hash Tables- object/dictionary(holds key/value pairs)

- Give it a word(key) and it will retieve the definition(value)
- hashing function spits out location
  - a value with key k is stored in the slot k. Using the hash function, we calculate the index of the table (slot) to which each value goes.

Stack/Queue- Stack(LIFO)- last in first out structure. Queue(FIFO)- first in first out

![Stack](https://miro.medium.com/v2/resize:fit:720/format:webp/1*QMifqahZm4DGQ91GkOhu4g.png)

- Peek: Return the top element of the stack without deleting it.
- isEmpty: Check if the stack is empty.
- isFull: Check if the stack is full.
- Push: Insert an element on to the top of the stack.
- Pop: Delete the topmost element and return it.

[Queue](https://miro.medium.com/v2/resize:fit:720/format:webp/1*K4-7c0lyUcSGRPmv3_9uqw.png)

- Enqueue: Insert an element to the end of the queue.
- Dequeue: Delete the element from the beginning of the queue.

DFS- dev first search
Graphs- nodes are pointing to nodes using edges. edges can have weights(numbers) asigned to them.
Tree- heirachical graph- data expand in one direction- 
BST- Binary Search Tree
-each node can have up to 2 children- one on either side(left/right) 
  left child < node  
  right child > node

The efficiency of operations performed on the data structure in the context of the problem requirements when deciding on which data structure to solve a problem. (Big O)

Recursive Case- calling the function inside the function
Base Case - when the function doesn't call itself
Have to tell the function when to stop recursing to prevent infinite recursive call stack.

## Souces

[Recursion](https://www.youtube.com/watch?v=vPEJSJMg4jY)
[Data Structures](https://www.youtube.com/watch?v=sVxBVvlnJsM)
[Big O](https://www.youtube.com/watch?v=v4cd1O4zkGw)
[Data Structures](https://towardsdatascience.com/8-common-data-structures-every-programmer-must-know-171acf6a1a42)