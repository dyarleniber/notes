[Back](../README.md)

## for and "while"

```golang
package main

import "fmt"

func main() {
	var sum int

	// Go has only one looping construct, the for loop.
	sum = 0
	for i := 0; i < 10; i++ {
		sum += i
	}
	fmt.Println(sum)

	// The init and post statements are optional.
	// For is Go's "while" - At that point you can drop the semicolons: C's while is spelled for in Go.
	sum = 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)

	// If you omit the loop condition it loops forever, so an infinite loop is compactly expressed.
	// for {
	// }

	// You can break the loop.
	for {
		fmt.Println("loop")
		break
	}

	// You can also continue to the next iteration of the loop.
	for n := 0; n <= 5; n++ {
		if n%2 == 0 {
			continue
		}
		fmt.Println(n)
	}
}
```
