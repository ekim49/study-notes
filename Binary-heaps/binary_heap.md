# Binary Heaps

Binary Heap has a **tree data structure**.

There are two types of binary heaps:

- Min-heap
- Max-heap

<br/>
<br/>

## Binary Heap Properties

Binary heaps must have these properties:

- Every parent node, at most, can have only **2 children**.
- Must be a **complete tree**.
  - It must be filled from left to right
  - **Every level must be full**, with the exception of the last level not needing to be full.
- Min-heap: Every parent's key must be **smaller** than its children nodes
  - Root node is always the smallest key within the heap
- Max-heap: Every parent's key must be **greater** than its children nodes. (opposite of min-heap)
  - Root node is always the largest within the heap

<img src="./complete_tree_1.png" width="600">
<img src="./complete_tree_2.png" width="600">
<img src="./incomplete_tree1.png" width="600">
<img src="./incomplete_tree2.png" width="600">

<br/>
<br/>

> **Heapify** is the process of creating a heap data structure from a binary tree. It is used to create a Min-Heap or a Max-Heap.

<br/>
<br/>

### Inserting within Min-Heap

- Swap parent node and children node if parent node is greater than children node.

### Removing Minimum Element

- Minimum element in Min-Heap will be the root node.
- Remove root node, then replace it with its children node.
- If root node is greater than children node, then swap.

<br/>
<br/>

## How to represent a heap using an array

To calculate the parent index of a given node:

```js
 parent index === Math.floor((index - 1) / 2)
```

To calculate the left child index of a given node:

```js
left child index === 2 * index + 1
```

To calculate the right child index of a given node:

```js
right child index === 2 * index + 2
```

<img src="./binary_heap_array.png" width="600">

### References

- https://www.youtube.com/watch?v=ifNlv0N5wT8&ab_channel=NoobCoder
- Heapify definition: https://www.programiz.com/dsa/heap-data-structure
