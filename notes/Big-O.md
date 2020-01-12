# Big-O (Asymptomatic analysis)

![Big-O Cheat Sheet Poster](https://www.bigocheatsheet.com/img/big-o-cheat-sheet-poster.png)

## What is good code? (3 pillars of programming)

- Readable (Clean code)
- Scalable
  - Speed (Time Complexity)
  - Memory (Space Complexity)

Most programming code solution there's usually a tradeoff between speed and memory.

Do you want things to go faster? Well, then you might have to sacrifice more memory.

Do you want less memory? Then you might have to sacrifice with increased speed.

Big O is what allows us to measure this idea of scalable code (Time Complexity and Space Complexity)

With Big-O we are worried about scale, and as things go larger and larger and larger.

## Types

- O(1)
  - Constant (No loops)
- O(log N)
  - Logarithmic (Usually searching algorithms have log n if they are sorted (Binary Search))
- O(n)
  - Linear (For loops, while loops through n items)
- O(n log(n))
  - Log Liniear (Usually sorting operations)
- O(n^2)
  - Quadratic (Every element in a collection needs to be compared to ever other element. Two nested loops)
- O(2^n)
  - Exponential (Recursive algorithms that solves a problem of size N)
- O(n!)
  - Factorial (You are adding a loop for every element)

> Iterating through half a collection is still O(n). Two separate collections: O(a * b)

## Rules

- Worst Case (O(1 + n) = O(n))
- Remove Constants (O(2n) = O(n))
- Different terms for inputs
  - Different inputs should have different variables
  - O(a + b) (Or O(a * b) if nested)

```javascript
  function compressBoxexTwice(boxes, boxes2) {
    boxes.forEach(function(boxes) {
      console.log(boxes);
    });

    boxes2.forEach(function(boxes) {
      console.log(boxes);
    });
  }
```

- Drop Non Dominants (O(n² + n) = O(n²))

## Memory (Space Complexity)

When a program executes it has two ways to remember things: The heap and the stack.

The heap is usually where we store variables that we assign values to and the stack is usually where we keep track of our function calls.

Is very similar to talking about the time cost. We simply look at the total size relative to the size of the input and see how many new variables or new memory we're allocating. How much memory is being used.

```javascript
function booooo(n) {
  for (let i = 0; i < n.length; i++) { // Create a new variable i just 1 time
    console.log('booooo');
  }
}

booooo([1,2,3,4,5]); // O(1)
```

```javascript
function arrayOfHiNTimes(n) {
  let hiArray = []; // Data Structure
  for (let i = 0; i < n; i++) { // Create a new variable i just 1 time
    hiArray[i] = 'hi'; // Add a new memory space n times
  }
  return hiArray;
}

arrayOfHiNTimes(5); // O(n)
```

### What causes Space complexity

- Variables
- Data Structures
- Function Call
- Allocations

## Premature optimization can be the root of all evil

### Readability

Readability is something that we are concerned with as well.

Sometimes readability Maybe matters more than scalability maybe time complexity is less important than space complexity.
And that's something that you want to be careful of.

Now with this newfound knowledge premature optimization can be the root of all evil.
It's a famous quote that a lot of developers know sometimes optimizing for time or space can negatively impact the readability of code.

So if you're working at a young startup for example might be more important for you to write code that's easy to ship and perhaps easy to understand later.

Perhaps not take as much time to write the code and think about the code and its implications for long term because maybe this startup has limited budget and needs things done fast.

That doesn't mean startups don't care about big-O analysis. A great engineer at a startup or at a big company knows how to strike the right balance between runtime space and of course readability.

If your function is linear time but the input is always let's say 7 items then the linear time algorithm might be better than the constant time algorithm so it really really depends on your situation.

### Scalability

Scaleable means we worry about large inputs.
Big-O is important to write scalable code, wich means think outside of just small inputs.
It means thinking long term, thinking big about your code and what could happen in the future.

All these methods which are functions have a cost associated with them. (Big-O cost).

We use big-O to measure why one data structure might be better than others.

Why should we use an array instead of let's say an object maybe object has better functions that we need for hard data.

We have different Big-O notation for different data structure and some data structures have really good search big-O some have insertions some have deletion.
And there's different pros and cons to each data structures.

Data structures are simply ways to store data and algorithms are simply functions or ways to use data structures to write our programs.

Pick the right data structure the right algorithms to write good programs.

Remember our two rules of good code readable and scalable, Big-O notation help us to make a decision of which data structure is going to be best.

We're going to use big-O to see what is a good solution to a problem and what is a bad solution to a problem in most interviews have this core concept.

What's the right data structure.

What's the right algorithm to write good programs.

Google hires engineers and developers that know this because they have a lot of scale that they have to think about.

A lot of inputs and people that know how to handle these programs are the ones that are going to be able to build great programs.

## Data Structures (Most used)

- Arrays
- Stacks
- Queues
- Linked Lists
- Trees
- Tries
- Graphs
- Hash Tables

## Algorithms (Most used)

- Sorting
- Dynamic Programming
- BFS + DFS (Searching)
- Recursion

## Links

- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/)
