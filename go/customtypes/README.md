[Back](../README.md)

## Custom types

```golang
package main

import (
	"fmt"
)

type customtype int

var x customtype = 100

func main() {
	y := 10

	fmt.Printf("%T \n", x)
	fmt.Printf("%T \n", y)
	
	// x = y // cannot use y (type int) as type customtype in assignment
	// y = x // cannot use x (type customtype) as type int in assignment
	
	x = 10
	x = customtype(y)
	y = 100
	y = int(x)
	
	fmt.Println(x, y)
}
```
