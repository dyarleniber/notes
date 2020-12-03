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
- [Go by Example](https://gobyexample.com/)

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

int  int8  int16  int32  int64 (default: int)
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64 (default: float64)

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

// an untyped constant takes the type needed by its context
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

## iota

```golang
package main

import (
	"fmt"
)

// Within a constant declaration, the predeclared identifier iota represents successive untyped integer constants.

const (
	a = iota
	b = iota
	c = iota
)

const (
	d = iota
	_ = iota
	e = iota
)

const (
	f = iota
	g
	h
)

const (
	i = iota * 10
	j
	k
)

func main() {
	fmt.Println(a, b, c, d, e, f, g, h, i, j, k)
}
```

```golang
package main

import (
	"fmt"
)

const (
	_  = iota             // 0
	KB = 1 << (iota * 10) // 1 << (1 * 10)
	MB = 1 << (iota * 10) // 1 << (2 * 10)
	GB = 1 << (iota * 10) // 1 << (3 * 10)
	TB = 1 << (iota * 10) // 1 << (4 * 10)
)

func main() {
	fmt.Println("binary\t\t\t\tdecimal")
	fmt.Printf("%b\t\t\t", KB)
	fmt.Printf("%d\n", KB)
	fmt.Printf("%b\t\t", MB)
	fmt.Printf("%d\n", MB)
	fmt.Printf("%b\t", GB)
	fmt.Printf("%d\n", GB)
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

## Strings

```golang
package main

import (
	"fmt"
)

func main() {
	// newline in string
	// syntax error: unexpected literal "; at end of statement
	// a := "A
	//
	// B";

	b := `A
	
	B`

	fmt.Println(b)

	x := "interpreted \n string \n\t literals \n" // interpreted string literals
	y := `raw \n\t string \n literals`            // raw string literals

	fmt.Println(x)
	fmt.Println(y) // output: raw \n\t string \n literals

	z := fmt.Sprint(x, y) // returns a new string

	fmt.Print(z)

}
```

### rune (int32)

```golang
package main

import (
	"fmt"
)

func main() {
	a := "e"
	b := "é"
	c := "香"
	fmt.Printf("%v, %v, %v\n", a, b, c)

	d := []byte(a)
	e := []byte(b)
	f := []byte(c)
	fmt.Printf("%v, %v, %v", d, e, f)
	// [101], [195 169], [233 166 153]

	// Each character in a UTF-8 string is a rune (int32)
	// 32 = 4 * 8, each UTF-8 character occupies 4 bytes of memory
	// The first character printed in the example before uses only 1 byte, the second one uses 2 bytes and the third one uses 3 bytes
	// UTF-8 characters can use 1 to 4 bytes
}
```

### Loop over strings

```golang
package main

import (
	"fmt"
)

func main() {
	s := "ascii éøâ 香"

	// For strings, the range loop iterates over Unicode code points
	for _, v := range s {
		fmt.Printf("%b - %T - %#U - %#x\n", v, v, v, v)
	}

	fmt.Println("")

	// To loop over individual bytes, simply use a normal for loop and string indexing
	for i := 0; i < len(s); i++ {
		fmt.Printf("%b - %T - %#U - %#x\n", s[i], s[i], s[i], s[i])
	}

	// %b	- binary value
	// %v	- the value in a default format
	// %#v	- a Go-syntax representation of the value
	// %T	- a Go-syntax representation of the type of the value
	// %#x  - hex value
	// %#U  - unicode value
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

## if

```golang
package main

import (
	"fmt"
	"math"
)

func sqrt(x float64) string {
	if x < 0 {
		return sqrt(-x) + "i"
	}
	return fmt.Sprint(math.Sqrt(x))
}

func pow(x, n, lim float64) float64 {
	// Like for, the if statement can start with a short statement to execute before the condition.
	// Variables declared inside an if short statement are also available inside any of the else blocks.
	if v := math.Pow(x, n); v < lim {
		return v
	} else {
		fmt.Printf("%g >= %g\n", v, lim)
	}
	// can't use v here, though
	return lim
}

func main() {
	if !(1 == 2) {
		fmt.Println("true")
	}

	fmt.Println(sqrt(2), sqrt(-4))

	fmt.Println(
		pow(3, 2, 10),
		pow(3, 3, 20),
	)
}
```

## switch

```golang
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	// Go only runs the selected case, not all the cases that follow. The break statement is provided automatically in Go.
	// Go's switch cases need not be constants, and the values involved need not be integers.

	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os {
	case "darwin":
		fmt.Println("OS X.")
	case "linux":
		fmt.Println("Linux.")
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.\n", os)
	}

	fmt.Println("When's Saturday?")
	today := time.Now().Weekday()
	switch time.Saturday {
	case today + 0:
		fmt.Println("Today.")
	case today + 1:
		fmt.Println("Tomorrow.")
	case today + 2:
		fmt.Println("In two days.")
	default:
		fmt.Println("Too far away.")
	}

	// Switch without a condition is the same as switch true.
	// This construct can be a clean way to write long if-then-else chains.
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}
}
```
