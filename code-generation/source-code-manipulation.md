---
title: 'Source Code Manipulation'
description: 'Learn how to perform source code manipulation in Go using go/ast package'
date: '2025-03-25'
category: 'Code Generation'
---

Source code manipulation in Go can be achieved using the `go/ast` and `go/parser` packages. These tools allow you to analyze or transform Go source code programmatically, providing powerful capabilities for developing code generators, linters, and formatting tools.

## Parsing Source Code

To manipulate Go source code, you first need to parse it into an Abstract Syntax Tree (AST). The ast represents the structured representation of the source code.

```go
package main

import (
	"fmt"
	"go/parser"
	"go/token"
)

func main() {
	src := `package main
import "fmt"
func main() {
    fmt.Println("Hello, World!")
}`

	fset := token.NewFileSet() // positions are relative to fset
	node, err := parser.ParseFile(fset, "", src, parser.AllErrors)
	if err != nil {
		fmt.Println(err)
		return
	}

	fmt.Printf("AST: %v\n", node)
}
```

## Inspecting the AST

Once parsed, you can traverse and inspect the AST to analyze or modify the source code.

```go
package main

import (
	"fmt"
	"go/ast"
	"go/parser"
	"go/token"
)

func main() {
	src := `package main
import "fmt"
func main() {
    fmt.Println("Hello, World!")
}`

	fset := token.NewFileSet()
	node, _ := parser.ParseFile(fset, "", src, parser.AllErrors)

	ast.Inspect(node, func(n ast.Node) bool {
		if n, ok := n.(*ast.BasicLit); ok {
			fmt.Printf("Found literal: %s at position %d\n", n.Value, fset.Position(n.Pos()))
		}
		return true
	})
}
```

## Modifying the AST

After inspecting the source, you may want to modify it. Here’s an example of replacing a string literal:

```go
package main

import (
	"go/ast"
	"go/parser"
	"go/printer"
	"go/token"
	"os"
)

func main() {
	src := `package main
import "fmt"
func main() {
    fmt.Println("Hello, World!")
}`

	fset := token.NewFileSet()
	node, _ := parser.ParseFile(fset, "", src, parser.AllErrors)

	ast.Inspect(node, func(n ast.Node) bool {
		if lit, ok := n.(*ast.BasicLit); ok && lit.Kind == token.STRING {
			lit.Value = `"Hello, Go!"`
		}
		return true
	})

	printer.Fprint(os.Stdout, fset, node)
}
```

## Best Practices

- Utilize `go/format` to format code after manipulation for better readability.
- Use `fset.Position()` for locating positions in the source code precisely, especially useful in larger files.

## Common Pitfalls

- Forgetting to check for node types while traversing can lead to runtime errors.
- Performing manipulations without validating AST nodes may lead to invalid Go syntax.
- Not accounting for imported packages while generating or manipulating code could result in undeclared errors.

## Performance Tips

- Avoid re-parsing the same code multiple times; work with the parsed AST.
- Use `ast.FilterFile` to focus on specific parts of the AST for efficiency.
- Cache file sets and nodes when manipulating multiple files to save processing time.