[Back](../README.md)

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
