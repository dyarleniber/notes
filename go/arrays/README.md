[Back](../README.md)

## Arrays

```golang
package main

import "fmt"

func main() {
	// The type [n]T is an array of n values of type T.
	// The size of an array is part of its type. The types [10]int and [20]int are distinct.
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1])
	fmt.Println(a)
	fmt.Println(len(a))

	// An array's length is part of its type, so arrays cannot be resized.
	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes)
}
```
