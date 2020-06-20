[Back](../README.md)

## Arrays

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
