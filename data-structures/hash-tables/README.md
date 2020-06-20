[Back](../README.md)

## Hash tables

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

### Hash table implementation in JavaScript

```javascript
class HashTable {
  constructor(size){
    this.data = new Array(size);
    // this.data = [];
  }

  _hash(key) {
    let hash = 0;
    for (let i =0; i < key.length; i++){
        hash = (hash + key.charCodeAt(i) * i) % this.data.length
    }
    return hash;
  }

  set(key, value) {
    let address = this._hash(key);
    if (!this.data[address]) {
      this.data[address] = [];
    } else {
      const currentBucket = this.data[address]
      for(let i = 0; i < currentBucket.length; i++){
        if(currentBucket[i][0] === key) {
          this.data[address][i][1] = value;
          return
        }
      }
    }
    this.data[address].push([key, value]);
  }

  get(key){
    const address = this._hash(key);
    const currentBucket = this.data[address]
    if (currentBucket) {
      for(let i = 0; i < currentBucket.length; i++){
        if(currentBucket[i][0] === key) {
          return currentBucket[i][1]
        }
      }
    }
    return undefined;
  }
  
  keys(){
    const keysArray = [];
    for (let i = 0; i < this.data.length; i++){
      if(this.data[i]){
        for (let j = 0; j < this.data[i].length; j++){
          keysArray.push(this.data[i][j][0])
        }
      }
    }
    return keysArray;
  }
}

const myHashTable = new HashTable(2);
myHashTable.set('grapes', 10000)
console.log(myHashTable.get('grapes'));
myHashTable.set('apples', 54)
myHashTable.set('apples', 55)
console.log(myHashTable.get('apples'))
myHashTable.set('oranges', 9)
console.log(myHashTable.get('oranges'))
console.log(myHashTable.keys());
```

### Hash tables vs Arrays

Hash tables are great when you want quick access to certain values. Searching through an array for an item takes a really long time, we have to loop through every item and see if a certain string or a certain number is in an array. With hash tables that is really fast.
Similarly inserting items in a hash table, unlike an array that might shift indexes, is typically O(1), you just simply have to hash and create the key.
Although we have those cases of collision most of the time that's something that we don't need to worry about too much, and we can assume and answered of O(1).

Hash tables is kind of like a hack on top of an array to let us use flexible keys instead of being stuck with 0, 1, 2, 3 and so on. However, hash tables are unordered. Keys aren't stored in a special order. If you're looking for the smallest key, the largest key, or all the keys in a range, you'll need to look through every key to find it.

#### Array

| Operation | Complexity |
| --------- | ---------- |
| search    | O(n)       |
| lookup    | O(1)       |
| push*     | O(1)       |
| insert    | O(n)       |
| delete    | O(n)       |

#### Hash table

| Operation | Complexity |
| --------- | ---------- |
| search    | O(1)       |
| insert    | O(1)       |
| lookup    | O(1)       |
| delete    | O(1)       |

### Pros and Cons

| Pros          | Cons               |
| --------------| -------------------|
| Fast lookups* | Unordered          |
| Fast inserts  | Slow key iteration |
| Flexible keys |                    |

###### `* Good collision resolution needed`

### Questions examples

- Given an array, create a function that returns the first recurring character.

```javascript
//Google Question
//Given an array = [2,5,1,2,3,5,1,2,4]:
//It should return 2

//Given an array = [2,1,1,2,3,5,1,2,4]:
//It should return 1

//Given an array = [2,3,4,5]:
//It should return undefined

function firstRecurringCharacter(input) {
  for (let i = 0; i < input.length; i++) {
    for (let j = i + 1; j < input.length; j++) {
      if(input[i] === input[j]) {
        return input[i];
      }
    }
  }
  return undefined
}

function firstRecurringCharacter2(input) {
  let map = {};
  for (let i = 0; i < input.length; i++) {
    if (map[input[i]]) {
      return input[i];
    }
    map[input[i]] = true;
  }
  return undefined;
}

firstRecurringCharacter2([1,5,5,1,3,4,6])

//Bonus... What if we had this:
// [2,5,5,2,3,5,1,2,4]
// return 5 because the pairs are before 2,2
```

### References

- https://www.interviewcake.com/concept/python/hash-map
- https://guide.freecodecamp.org/computer-science/data-structures/hash-tables/
- https://en.wikipedia.org/wiki/Hash_table
- https://en.wikipedia.org/wiki/Comparison_of_programming_languages_(associative_array)
