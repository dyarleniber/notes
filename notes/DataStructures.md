# Data structures

Data structure is a collection of values and algorithms are the steps or processes we put into place to manipulate these collection of values. It's what allows us to write programs.

A person who knows how data structures and algorithms work and how to use them can write programs great programs.

The most used data structures

- Arrays
- Stacks
- Queues
- Linked Lists
- Trees
- Tries
- Graphs
- Hash Tables

The values in a data structure can have relationships among them and they can have functions applied to them. Each one is different in what it can do and what it is best used for.

The most important thing to take away is that each data structure is good and is specialized for its own thing.

There are two parts to understanding Data Structures

- How to Build One
- How to use it

> The second part is usually most importante, because data structure are usually just tools and most of the time they are already pre-built for us.

Each data structure has their tradeoffs, some are good at certain operations and others are good at other operations.

![Big-O Cheat Sheet Poster](https://www.bigocheatsheet.com/img/big-o-cheat-sheet-poster.png)

## Arrays

Arrays, which are sometimes called Lists, organizes items sequentially. That means one after another in memory.

| lookup | O(1) |
| push | O(1) |
| insert | O(n) |
| delete | O(n) |

```javascript
const strings = ['a', 'b', 'c', 'd']; // 4*4 = 16 bytes of storage in a 32bits system

// push
strings.push('e'); // O(1)

// pop 
strings.pop(); // O(1)

// unshift
strings.unshift('x'); // O(n)

// splice
strings.splice(2, 0, 'x'); // O(n)
```

## Links  

- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
