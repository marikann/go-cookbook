---
title: 'Parsing Text with the Parser Package'
description: 'Learn how to parse structured text in Go using the text/scanner package'
date: '2025-03-24'
category: 'Standard library packages'
---

Go provides a robust way to parse structured text through the `text/scanner` package. This is particularly useful when you need a lightweight way to process text or create simple interpreters.

## Basic Text Scanning

Here's a simple example demonstrating how to use the `text/scanner` package to tokenize an input string.

```go
package main

import (
	"fmt"
	"text/scanner"
	"strings"
)

func main() {
	var s scanner.Scanner
	s.Init(strings.NewReader("42 + (1337 / foo)"))
	
	for tok := s.Scan(); tok != scanner.EOF; tok = s.Scan() {
		fmt.Printf("%s: %q\n", s.Position, s.TokenText())
	}
}
```

## Handling Tokens with Scanning

You can further manipulate how tokens are handled using the `text/scanner` package:

```go
package main

import (
	"fmt"
	"text/scanner"
	"strings"
)

func main() {
	var s scanner.Scanner
	s.Init(strings.NewReader("var x = 23; if x > 10 { x = x + 1 }"))
	s.Mode ^= scanner.SkipComments // include comments when scanning

	for tok := s.Scan(); tok != scanner.EOF; tok = s.Scan() {
		switch tok {
		case scanner.Ident:
			fmt.Printf("Identifier: %s\n", s.TokenText())
		case scanner.Int:
			fmt.Printf("Integer: %s\n", s.TokenText())
		default:
			fmt.Printf("Other: %s\n", s.TokenText())
		}
	}
}
```

## Configuring Scanner Parameters

The `scanner.Scanner` type offers several configuration options:

- `Mode`: Defines what types of tokens to recurse for.
- `Position`: Allows detailed information about token positions.

## Best Practices

- Utilize `s.Init()` with different types of readers based on your input source.
- Configure `Mode` to customize tokenization strategies to best suit your text parsing needs.
- Make use of `s.Error` to handle syntax errors gracefully rather than crashing the application.

## Common Pitfalls

- Ignoring scanner errors can lead to silent parsing failures.
- Not considering whitespace or comments that might affect parsing.
- Assuming the same token type for all similar tokens (e.g., identifiers vs reserved words).

## Performance Tips

- Reuse `scanner.Scanner` to avoid repeated allocations, especially in loops or large-scale text processing.
- Adjust `Mode` to skip undesired tokens to reduce overhead during scanning.
- Profile your parsing logic to identify bottlenecks if dealing with large texts or complex parsing logic.