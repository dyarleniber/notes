[Back](../README.md)

## Variables

```golang
package main

import (
	"fmt"
)

// the var statement declares variables
// it can be used anywhere in the code (package or function level)
var a int // variables declared without an explicit initial value are given their zero value
var b, c bool

// a = 2 // syntax error: non-declaration statement outside function body

func main() {
	var d, e int = 1, 2 // a var declaration can include initializers
	var f, g = "f", "g" // if a initializer is present, the type can be omitted

	fmt.Println(a, b, c, d, e, f, g)

	// just inside a function the short assignment statement can be used in place of a var declaration
	h := true
	i, j, k := true, 1, "k"

	fmt.Println(h, i, j, k)

	// the short assignment statement must have at least one new variable, but it can be used to assign a new value to an existing variable
	k, l := "k.", false

	fmt.Println(k, l)
}
```
