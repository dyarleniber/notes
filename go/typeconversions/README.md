[Back](../README.md)

## Type conversions

```golang
package main

import (
	"fmt"
)

func main() {
	// T(v) converts the value v to the type T
	// In go, assignments between different types requires an explicit type conversion

	var i int = 55
	var f float64 = float64(i)
	var u uint = uint(f)

	fmt.Println(i, f, u)

	in := 55
	fl := float64(i)
	ui := uint(f)

	fmt.Println(in, fl, ui)
}
```
