---
title: 'Token Package'
description: 'Explore the token package in Go for tokenizing text.'
date: '2025-03-24'
category: 'Standard library packages'
---

The `token` package in Go is primarily used with the Go `go/parser` package. It provides facilities to tokenize Go source code, which is crucial for building tools like linters, compilers, or code analysis tools. However, it can also be creatively used for custom tokenization needs.

## Basic Tokenization with go/parser

Here's an example of using the `token` package alongside `go/parser` to tokenize a Go source file:

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
	fmt.Println("Hello, world!")
}`

	// Create a new file set.
	fset := token.NewFileSet()

	// Parse the source code.
	f, err := parser.ParseFile(fset, "source.go", src, parser.AllErrors)
	if err != nil {
		fmt.Println(err)
		return
	}

	// Iterate over the tokens.
	for _, decl := range f.Decls {
		fmt.Printf("%T\n", decl)
	}
}
```

## Tokenizing Go Source Code

Below demonstrates how to tokenize source code and print the position and literal value of each token:

```go
package main

import (
	"fmt"
	"go/scanner"
	"go/token"
)

func main() {
	src := []byte(`package main

import "fmt"

func main() {
	fmt.Println("Hello, world!")
}`)

	// Initialize the source file.
	fset := token.NewFileSet()  
	file := fset.AddFile("example.go", fset.Base(), len(src))

	// Initialize the scanner.
	var s scanner.Scanner
	s.Init(file, src, nil, scanner.ScanComments)

	// Scan the tokens.
	for {
		pos, tok, lit := s.Scan()
		if tok == token.EOF {
			break
		}
		fmt.Printf("%s\t%s\t%q\n", fset.Position(pos), tok, lit)
	}
}
```

## Best Practices

- **Use `NewFileSet` for Each Session**: Ensure each parsing session uses a new `FileSet` to avoid conflicts and incorrect positions when dealing with multiple source files.
- **Error Handling**: When using `parser.ParseFile`, always handle potential errors gracefully.
- **Token Streaming**: Use a streaming approach (like `scanner.Scanner`) instead of reading everything into memory if dealing with large codebases.

## Common Pitfalls

- **Ignoring Import Paths**: If modifying or analyzing code, ensure to handle import paths correctly to avoid resolution issues.
- **Literal vs. Token Misunderstanding**: Be cautious about the difference between a token (like `token.IDENT`) and its literal string representation (`lit`).
- **Not Handling Newlines**: Ignoring newline tokens during scanning may lead to incorrect assumptions about code structure.

## Performance Tips

- **Limit Memory Usage**: When processing large source files, prefer streaming methods and avoid loading unnecessary code parts into memory.
- **Optimize Scans**: Use partial scanning for specific needs when full file parsing isn't required.
- **Cache FileSets**: Reuse `FileSet` instances where applicable to reduce overhead in cases where source structure may remain consistent.