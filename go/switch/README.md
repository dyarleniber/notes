[Back](../README.md)

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

	// Case list
	switch '\n' {
	case ' ', '\t', '\n', '\f', '\r':
		fmt.Println("true")
	default:
		fmt.Println("false")
	}

	// A fallthrough statement transfers control to the next case.
	switch 2 {
	case 1:
		fmt.Println("1")
		fallthrough
	case 2:
		fmt.Println("2")
		fallthrough
	case 3:
		fmt.Println("3")
	}
}
```
