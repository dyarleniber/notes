[Back](../README.md)

## range

```golang
package main

import (
	"fmt"
)

func main() {
	slice := []int{20, 21, 22, 23, 1, 13}

	total := 0

	for _, valor := range slice {
		// mesma coisa que total = total + valor
		total += valor
	}

	fmt.Println("O valor total é:", total)

	// without range
	for i := 0; i < len(sabores); i++ {
		fmt.Println(sabores[i])
	}
}
```
