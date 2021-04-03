[Back](../README.md)

## Slices

```golang
package main

import (
	"fmt"
)

func main() {
	// An array has a fixed size. A slice, on the other hand, is a dynamically-sized
	// In practice, slices are much more common than arrays.

	// []T
	// a[low : high] - This selects a half-open range which includes the first element, but excludes the last one.

	array := [5]int{1, 2, 3, 4, 5}
	fmt.Println(array)

	slice := []int{1, 2, 3, 4, 5}
	fmt.Println(slice)

	// first argument to append must be slice
	// array = append(array, 6)

	slice = append(slice, 6, 7, 8)
	fmt.Println(slice)

	slice[3] = 348756
	// index out of range [20] with length 5
	// slice[20] = 1
	fmt.Println(slice[3])


	flavors := []string{"Margherita", "Margherita", "BBQ", "Pineapple", "Hawaiian"}

	// all elements
	s := flavors[:]
	fmt.Println(s)

	s = flavors[2:4]
	fmt.Println(s)


	flavors = append(flavors[:2], flavors[4:]...)
	fmt.Println(flavors)
}
```

```golang
package main

import (
	"fmt"
)

func main() {
	// SLICES ARE LIKE REFERENCES TO ARRAYS (UNDERLYING ARRAYS)
	// Changing the elements of a slice modifies the corresponding elements of its underlying array

	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names)

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b)

	b[0] = "XXX"
	fmt.Println(a, b)
	fmt.Println(names)
}
```

```golang
package main

import (
	"fmt"
)

func main() {
	firstslice := []int{1, 2, 3, 4, 5}

	// [1 2 3 4 5]
	fmt.Println(firstslice)

	secondslice := append(firstslice[:2], firstslice[4:]...)

	// [1 2 5]
	fmt.Println(secondslice)

	// [1 2 5 4 5]
	fmt.Println(firstslice)
}
```

### Slices of slices

```golang
package main

import (
	"fmt"
)

func main() {
	ss := [][]int{
		[]int{1, 2, 3, 4, 5, 6},
		[]int{7, 8, 9, 10, 11, 12},
		[]int{13, 14, 15, 16, 17, 18},
	}
	fmt.Println(ss[2][4])
}
```

### Nil slices

```golang
package main

import "fmt"

func main() {
	// The zero value of a slice is nil
	var s []int
	fmt.Println(s, len(s), cap(s))
	if s == nil {
		fmt.Println("nil!")
	}
}
```

### make

```golang
package main

import (
	"fmt"
)

func main() {
	// The make function allocates a zeroed array and returns a slice that refers to that array:

	// The length of a slice is the number of elements it contains

	// The capacity of a slice is the number of elements in the underlying array

	slice := make([]int, 5, 10) // len(b)=5, cap(b)=10

	fmt.Println(slice) // [0 0 0 0 0]

	slice[0], slice[1], slice[2], slice[3], slice[4] = 1, 2, 3, 4, 5

	slice = append(slice, 6)
	slice = append(slice, 7)
	slice = append(slice, 8)
	slice = append(slice, 9)
	slice = append(slice, 10)

	// [1 2 3 4 5 6 7 8 9 10] 10 10
	fmt.Println(slice, len(slice), cap(slice))

	slice = append(slice, 10)

	// [1 2 3 4 5 6 7 8 9 10 10] 11 20
	fmt.Println(slice, len(slice), cap(slice))
}
```
