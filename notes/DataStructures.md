# Data structures

Simply put, a data structure is a container that stores data in a specific layout. This “layout” allows a data structure to be efficient in some operations and inefficient in others. Your goal is to understand data structures so that you can pick the data structure that’s most optimal for the problem at hand.

Data structure is a collection of values and algorithms are the steps or processes we put into place to manipulate these collection of values. It's what allows us to write programs.

A person who knows how data structures and algorithms and how to use them, can write programs great programs.

No matter what problem are you solving, in one way or another you have to deal with data — whether it’s an employee’s salary, stock prices, a grocery list, or even a simple telephone directory.

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


## Computer memory

Given that we are always going to store our data on a computer, it makes sense for us to find out a little bit about how that information is stored.

Things like variables, numbers, or arrays are stored in what we call random access memory or RAM for short. In RAM you lose the memory when the computer turns off. However, RAM is faster than the hard drive.

![RAM](https://i.ibb.co/XzDwcCC/RAM.jpg)

![RAM2](https://i.ibb.co/KsFXGKw/DS.jpg)

### Bits, bytes, and words

The most fundamental unit of computer memory is the bit, it can only take one of two values: it is either “on” or “off”.

A collection of 8 bits is called a byte and (on the majority of computers today) a collection of 4 bytes, or 32 bits, is called a word. At the lowest level, any data stored on a computer is just a large collection of bits, a data set is just a series of zeroes and ones.

The number of bytes and words used for an individual data value will vary depending on the storage format, the operating system, and even the computer hardware, but in many cases, a single letter or character of text takes up one byte and an integer, or whole number, takes up one word. A real or decimal number takes up one or two words depending on how it is stored.

For example, the text “hello” would take up 5 bytes of storage, one per character. The text “12345” would also require 5 bytes. The integer 12,345 would take up 4 bytes (1 word), as would the integers 1 and 12,345,678. The real number 123.45 would take up 4 or 8 bytes, as would the values 0.00012345 and 12345000.0.

![Bits, bytes, and words](http://statmath.wu.ac.at/courses/data-analysis/itdtHTML/img13.png)

### Binary, Octal, and Hexadecimal

#### Binary

A piece of computer memory can be represented by a series of 0's and 1's, with one digit for each bit of memory; the value 1 represents an “on” bit and a 0 represents an “off” bit. This notation is described as binary form. For example, below is a single byte of memory that contains the letter 'A' (ASCII code 65; binary 1000001).

#### Octal

A single word of memory contains 32 bits, so it requires 32 digits to represent a word in binary form. A more convenient notation is octal, where each digit represents a value from 0 to 7. Each octal digit is the equivalent of 3 binary digits, so a byte of memory can be represented by 3 octal digits.

Binary values are pretty easy to spot, but octal values are much harder to distinguish from normal decimal values, so when writing octal values, it is common to precede the digits by a special character, such as a leading '0'.

As an example of octal form, the binary code for the character 'A' splits into triplets of binary digits (from the right) like this: 01 000 001. So the octal digits are 101, commonly written 0101 to emphasize the fact that these are octal digits.

#### Hexadecimal

An even more efficient way to represent memory is hexadecimal form. Here, each digit represents a value between 0 and 16, with values greater than 9 replaced with the characters a to f. A single hexadecimal digit corresponds to 4 bits, so each byte of memory requires only 2 hexadecimal digits. As with octal, it is common to precede hexdecimal digits with a special character, e.g., 0x or #. The binary form for the character 'A' splits into two quadruplets: 0100 0001. The hexadecimal digits are 41, commonly written 0x41 or #41.

Another standard practice is to write hexadecimal representations as pairs of digits, corresponding to a single byte, separated by spaces. For example, the memory storage for the text “just testing” (12 bytes) could be represented as follows:

6a 75 73 74 20 74 65 73 74 69 6e 67

When displaying a block of computer memory, another standard practice is to present three columns of information: the left column presents an offset, a number indicating which byte is shown first on the row; the middle column shows the actual memory contents, typically in hexadecimal form; and the right column shows an interpretation of the memory contents (either characters, or numeric values). For example, the test “just testing” is shown below complete with offset and character display columns.

0  :  6a 75 73 74 20 74 65 73 74 69 6e 67  |  just testing

#### References

- http://statmath.wu.ac.at/courses/data-analysis/itdtHTML/node55.html
- https://www.freecodecamp.org/news/the-top-data-structures-you-should-know-for-your-next-coding-interview-36af0831f5e3/


## ARRAYS

Arrays, which are sometimes called Lists, organizes items sequentially. That means one after another in memory. An array is the simplest and most widely used data structure. Other data structures like stacks and queues are derived from arrays.

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

### Array implementation in JavaScript

```javascript
class MyArray {
  constructor() {
    this.length = 0;
    this.data = {};
  }
  get(index) {
    return this.data[index];
  }
  push(item) {
    this.data[this.length] = item;
    this.length++;
    return this.data;
  }
  pop() {
    const lastItem = this.data[this.length - 1];
    delete this.data[this.length - 1];
    this.length--;
    return lastItem;
  }
  deleteAtIndex(index) {
    const item = this.data[index];
    this.shiftItems(index);
    return item;
  }
  shiftItems(index) {
    for (let i = index; i < this.length - 1; i++) {
      this.data[i] = this.data[i + 1];
    }
    console.log(this.data[this.length - 1]);
    delete this.data[this.length - 1];
    this.length--;
  }
}

const myArray = new MyArray();
myArray.push('hi');
myArray.push('you');
myArray.push('!');
myArray.pop();
myArray.deleteAtIndex(0);
myArray.push('are');
myArray.push('nice');
myArray.shiftItems(0);
console.log(myArray);
```

### Pros and Cons

| Pros            | Cons                               |
| --------------- | ---------------------------------- |
| Fast lookups    | Slow inserts                       |
| Fast push / pop | Slow deletes                       |
| Ordered         | Fixed size (if using static array) |

### Questions example

- [Google interview example](https://www.youtube.com/watch?v=XKu_SEDAykw)

```javascript
// Naive
function hasPairWithSum(arr, sum){
  var len = arr.length;
  for(var i =0; i<len-1; i++){
     for(var j = i+1;j<len; j++){
        if (arr[i] + arr[j] === sum)
            return true;
     }
  }

  return false;
}

// Better
function hasPairWithSum2(arr, sum){
  const mySet = new Set();
  const len = arr.length;
  for (let i = 0; i < len; i++){
    if (mySet.has(arr[i])) {
      return true;
    }
    mySet.add(sum - arr[i]);
  }
  return false;
}

hasPairWithSum2([6,4,3,2,1,7], 9)
```

- Given 2 arrays, create a function that let's a user know (true/false) whether these two arrays contain any common items.

```javascript
// Given 2 arrays, create a function that let's a user know (true/false) whether these two arrays contain any common items

// For example:
// const array1 = ['a', 'b', 'c', 'x'];
// const array2 = ['z', 'y', 'i'];
// should return false.

// const array1 = ['a', 'b', 'c', 'x'];
// const array2 = ['z', 'y', 'x'];
// should return true.

// 2 parameters - arrays - no size limit
// return true or false

function containsCommonItem(arr1, arr2) {
  for (let i=0; i < arr1.length; i++) {
    for ( let j=0; j < arr2.length; j++) {
      if(arr1[i] === arr2[j]) {
        return true;
      }
    }
  }
  return false
}

//O(a*b)
//O(1) - Space Complexity

const array1 = ['a', 'b', 'c', 'x'];
const array2 = ['z', 'y', 'a'];

function containsCommonItem2(arr1, arr2) {
  // loop through first array and create object where properties === items in the array
  // can we assume always 2 params?

  let map = {};
  for (let i=0; i < arr1.length; i++) {
    if(!map[arr1[i]]) {
      const item = arr1[i];
      map[item] = true;
    }
  }
  // loop through second array and check if item in second array exists on created object. 
  for (let j=0; j < arr2.length; j++) {
    if (map[arr2[j]]) {
      return true;
    }
  }
  return false
}

//O(a + b) Time Complexity
//O(a) Space Complexity

// containsCommonItem2(array1, array2)

function containsCommonItem3(arr1, arr2) {
  return arr1.some(item => arr2.includes(item))
}

containsCommonItem(array1, array2)
containsCommonItem2(array1, array2)
containsCommonItem3(array1, array2)
```

- Create a function that reverses a string.

```javascript
// Create a function that reverses a string

function reverse(str){
  if(!str || typeof str != 'string' || str.length < 2 ) return str;
  
  const backwards = [];
  const totalItems = str.length - 1;
  for(let i = totalItems; i >= 0; i--){
    backwards.push(str[i]);
  }
  return backwards.join('');
}

function reverse2(str){
  //check for valid input
  return str.split('').reverse().join('');
}

const reverse3 = str => [...str].reverse().join('');

reverse('Timbits Hi')
reverse('Timbits Hi')
reverse3('Timbits Hi')
```

- Create a function that merge a sorted array.

```javascript
function mergeSortedArrays(array1, array2){
  const mergedArray = [];
  let array1Item = array1[0];
  let array2Item = array2[0];
  let i = 1;
  let j = 1;
  
  //We should actually move these 2 if statements to line 2 so that we do the checks before we do assignments in line 3 and 4!
  if(array1.length === 0) {
    return array2;
  }
  if(array2.length === 0) {
    return array1;
  }

  while (array1Item || array2Item){
   if(array2Item === undefined || array1Item < array2Item){
     mergedArray.push(array1Item);
     array1Item = array1[i];
     i++;
   }   
   else {
     mergedArray.push(array2Item);
     array2Item = array2[j];
     j++;
   }
  }
  return mergedArray;
}

mergeSortedArrays([0,3,4,31], [3,4,6,30]);
```

- Other questions.
	- [Two Sum](https://leetcode.com/problems/two-sum/description/)
	- [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)
	- [Move Zeroes](https://leetcode.com/problems/move-zeroes/description/)
	- [Contains Duplicate](https://leetcode.com/problems/contains-duplicate/description/)
	- [Rotate Array](https://leetcode.com/problems/rotate-array/description/)
	- [Longest Word - This question can be done using arrays, or even regular expressions](https://www.coderbyte.com/language/Longest%20Word)


## HASH TABLE

Hash table is a data structure that can map keys to values.A hash table uses a hash function to compute an index into an array of buckets, from which the desired values can be found.Time complexity of a well defined Hash function can be O(1).

![Hash table example](https://upload.wikimedia.org/wikipedia/commons/thumb/7/7d/Hash_table_3_1_1_0_1_0_0_SP.svg/315px-Hash_table_3_1_1_0_1_0_0_SP.svg.png)

Some Important properties of Hash Table:
  - Values are not stored in sorted order.
  - In a hash table, one must also handle potential collisions. This is often done by chaining, which means to create a linked list of all the values whose keys map to a particular index.

A hash table is traditionally implemented with an array of linked lists. When we want to insert a key/Value pair, we map the key to an index in the array using the hash function. The value is then inserted into the linked list at that position.

### Hash Collisions

When you are using a hash map you have to assume that hash collisions are unavoidable, since you will be using a hash map which is significantly smaller in size than the amount of data you have. The two main approaches to solving these collisions are Chaining and Open Addressing.

#### Chaining

One way you can resolve hash collisions is using chaining. What this means is for each key-value mapping in the hash table, the value field will not hold only one cell of data, but rather a linked list of data.
The major setback regarding chaining is the increase in time complexity. This means, that instead of the O(1) properties of a regular hash table, each action will now take greater time as we need to traverse the linked list.

![Hash collision by separate chaining](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Hash_table_5_0_1_1_1_1_0_LL.svg/500px-Hash_table_5_0_1_1_1_1_0_LL.svg.png)

#### Open Addressing

Another way you can resolve hash collisions is using open addressing. In this method once a value is mapped to a key that is already occupied, you move along the adjacent keys of the hash table in a preordained determined fashion, until you find a key with an empty value. 
The major setback of open addressing lies in the fact that when needing to look for values, they might not be in the place you expect them to be (the key mapping). Therefore you have to traverse parts of the hash table in order to find the value you are looking for, thus resulting in increased time complexity.

![Hash collision resolved by open addressing](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Hash_table_5_0_1_1_1_1_0_SP.svg/380px-Hash_table_5_0_1_1_1_1_0_SP.svg.png)

#### Time Complexity

It is very important to note that hash tables have amortised constant complexity i.e. on an average case the complexity will be O(1). In worst case, If too many elements were hashed into the same key, it can have a time complexity of O(n).

#### Visualization 

- https://www.cs.usfca.edu/~galles/visualization/OpenHash.html

### References

- https://guide.freecodecamp.org/computer-science/data-structures/hash-tables/
- https://en.wikipedia.org/wiki/Hash_table
- https://en.wikipedia.org/wiki/Comparison_of_programming_languages_(associative_array)


## Links

- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
