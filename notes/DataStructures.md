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
There are actually two types of arrays. One is called static and the other dynamic array.
The one limitation of static array is that they're fixed in size, meaning you need to specify the number of elements.

With static arrays, there's no guarantee that after we've allocated shelfs of memory that you can keep adding things on.
We solve this problem with dynamic arrays, it allow us to copy and rebuild an array at a new location which with more memory.
If we wanted more memory, if we realize that we forgot another item on our list for example. What happens is we copy the entire array, we allocate new blocks of memory and paste the list plus the new item into that new location.

> Most of time, and during interviews you will be talking about dynamic arrays and you will not have to worry about allocating memory and thinking about the possibility that you might have to copy the array.
> When it comes to arrays, just because you are adding at the end using the push command, you can assume that it is O(1) most of the times.

### Static Array

| Operation | Complexity |
| --------- | ---------- |
| lookup    | O(1)       |
| push      | O(1)       |
| insert    | O(n)       |
| delete    | O(n)       |

### Dynamic Array

| Operation | Complexity |
| --------- | ---------- |
| lookup    | O(1)       |
| append*   | O(1)       |
| insert    | O(n)       |
| delete    | O(n)       |

###### `* Can be O(n)`

> In javascript and other languages like Python, or Java, they work like a dynamic array. They automatically allocate memory according to the increase in size of the array.

```c++
// c++

int a[4];
```

```javascript
// javascript

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
