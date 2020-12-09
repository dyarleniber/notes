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


	sabores := []string{"pepperoni", "mozzarela", "abacaxi", "quatroqueijos", "marg"}

	// all elements
	fatia := sabores[:]
	fmt.Println(fatia)

	fatia = sabores[2:4]
	fmt.Println(fatia)


	sabores = append(sabores[:2], sabores[4:]...)
	fmt.Println(sabores)
}
```

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

```golang
package main

import (
	"fmt"
)

func main() {
	primeiroslice := []int{1, 2, 3, 4, 5}

	// [1 2 3 4 5]
	fmt.Println(primeiroslice)

	segundoslice := append(primeiroslice[:2], primeiroslice[4:]...)

	// [1 2 5]
	fmt.Println(segundoslice)

	// [1 2 5 4 5]
	fmt.Println(primeiroslice)
}
```

### make

```golang
package main

import (
	"fmt"
)

func main() {
	slice := make([]int, 5, 10)

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
