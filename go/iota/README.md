[Back](../README.md)

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
