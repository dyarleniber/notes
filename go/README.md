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

- [Basic types](basictypes/README.md)
- [Variables](variables/README.md)
- [Constants](constants/README.md)
- [iota](iota/README.md)
- [Type conversions](typeconversions/README.md)
- [Overflow](overflow/README.md)
- [Strings](strings/README.md)
- [Custom types](customtypes/README.md)
- [for and "while"](forandwhile/README.md)
- [if](if/README.md)
- [switch](switch/README.md)
- [Arrays](arrays/README.md)
- [Slices](slices/README.md)
- [range](range/README.md)
