[Back](../README.md)

# Go

Go is a statically and strongly typed, compiled programming language developed by a team at Google and many contributors from the open source community.
The language is often referred to as "Golang" because of its domain name, golang.org, but the proper name is Go.

Useful links:

- [Go official website](https://golang.org/)
- [Getting Started](https://golang.org/doc/install)
- [A Tour of Go](https://tour.golang.org/list)
- [How to Write Go Code](https://golang.org/doc/code.html)
- [Effective Go](https://golang.org/doc/effective_go.html)
- [Packages](https://golang.org/pkg/)
- [Command Documentation](https://golang.org/doc/cmd)
- [Package testing](https://golang.org/pkg/testing/)

## Hello World

Programs start running in package main.

```golang
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello World!")
}
```

> In Go, exported names must start with a capital letter, otherwise they will be private names (not accessible outside the package).

> It is good style to use the "factored" import statement (when the code groups the imports into a parenthesized).

## Basic types

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```

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

## Constants

```golang
package main

import (
	"fmt"
)

const Pi = 3.14

// numeric constants are high-precision values
const (
	// create a huge number by shifting a 1 bit left 100 places
	// in other words, the binary number that is 1 followed by 100 zeroes
	Big = 1 << 100
	// shift it right again 99 places, so we end up with 1<<1, or 2
	Small = Big >> 99
)

func needInt(x int) int { return x*10 + 1 }
func needFloat(x float64) float64 {
	return x * 0.1
}

func main() {
	// an untyped constant takes the type needed by its context
	fmt.Println(needInt(Small))
	fmt.Println(needFloat(Small))
	fmt.Println(needFloat(Big))
	// fmt.Println(needInt(Big)) // overflows int
}
```

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

## Strings

```golang
package main

import (
	"fmt"
)

func main() {
	x := "interpreted \n string \n\t literals \n" // interpreted string literals
	y := `raw \n\t string \n literals`            // raw string literals

	fmt.Println(x, y)

	z := fmt.Sprint(x, y) // returns a new string

	fmt.Print(z)
}
```

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
