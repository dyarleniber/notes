[Back](../README.md)

## Linked lists

A linked list is a linear collection of data elements, whose order is not given by their physical placement in memory. Instead, each element points to the next. It is a data structure consisting of a collection of nodes which together represent a sequence.
It is a simple data structure, but it can be used to implement more complicated Data Structures like Queues, Stacks, etc. There are three main types of Linked Lists:

- Simple Linked List
- Doubly Linked List (or Double Ended Linked List)
- Circular Linked Lists (Ring Buffer)

> Each record of a linked list is often called an 'element' or 'node'. The field of each node that contains the address of the next node is usually called the 'next link' or 'next pointer'. The remaining fields are known as the 'data', 'information', 'value', 'cargo', or 'payload' fields.

> The 'head' of a list is its first node. The 'tail' of a list may refer either to the rest of the list after the head, or to the last node in the list.

- [Linked list visualization](https://visualgo.net/en/list?slide=1)

### Types of linked lists

#### Singly linked list

Simple Linked List or Singly linked lists contain nodes which have a data field as well as 'next' field, which points to the next node in line of nodes. Operations that can be performed on singly linked lists include insertion, deletion and traversal.

> Traversal refers to the process of visiting (checking and/or updating) each node in a data structure.

![Simple Linked List example](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Singly-linked-list.svg/408px-Singly-linked-list.svg.png)

#### Doubly linked list

In a Doubly linked list, each node contains, besides the next-node link, a second link field pointing to the 'previous' node in the sequence. The two links may be called 'forward('s') and 'backwards', or 'next' and 'prev'('previous').

![Doubly linked list](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5e/Doubly-linked-list.svg/610px-Doubly-linked-list.svg.png)

#### Circular linked list

In the last node of a list, the link field often contains a null reference, a special value is used to indicate the lack of further nodes. A less common convention is to make it point to the first node of the list; in that case, the list is said to be 'circular' or 'circularly linked'; otherwise, it is said to be 'open' or 'linear'. It is a list where the last pointer points to the first node.

![Circular linked list](https://upload.wikimedia.org/wikipedia/commons/thumb/d/df/Circularly-linked-list.svg/350px-Circularly-linked-list.svg.png)

### References

- https://en.wikipedia.org/wiki/Linked_list
- https://guide.freecodecamp.org/computer-science/data-structures/hash-tables/
