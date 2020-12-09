[Back](../README.md)

## Overflow

```golang
package main

import (
	"fmt"
)

func main() {
	var i uint16
	// i = 65536 // constant 65536 overflows uint16
	i = 65535
	fmt.Println(i) // 65535
	i++
	fmt.Println(i) // 0
	i++
	fmt.Println(i) // 1
}
```
