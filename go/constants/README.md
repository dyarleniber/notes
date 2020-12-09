[Back](../README.md)

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
