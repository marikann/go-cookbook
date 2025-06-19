---
title: 'strconv Package in Go'
description: 'Learn how to use the strconv package in Go for conversions between strings and other types.'
date: '2025-03-24'
category: 'Standard library packages'
---

The `strconv` package in Go provides functions to convert data types to and from string representations. It is particularly useful when handling input/output or preparing data for serialization.

## Converting String to Integer

To convert strings to integers, use the `strconv.Atoi` function for base 10 or `strconv.ParseInt` for more control over the base.

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// Basic conversion from string to integer.
	number, err := strconv.Atoi("123")
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println("Converted Integer:", number)
	}

	// Parsing with a specified base.
	number64, err := strconv.ParseInt("7B", 16, 64) // Hexadecimal to int
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Println("Parsed Integer:", number64)
	}
}
```

## Converting String to Float

To convert strings to floating-point numbers, use `strconv.ParseFloat`.

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	floatNum, err := strconv.ParseFloat("123.456", 64)
	if err != nil {
		fmt.Println("Error:", err)
	} else {
		fmt.Printf("Converted Float: %f\n", floatNum)
	}
}
```

## Converting Integer to String

Use `strconv.Itoa` or `strconv.FormatInt` to convert an integer to a string.

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// Convert integer to string.
	str := strconv.Itoa(123)
	fmt.Println("String:", str)

	// Format with a specific base.
	strBase := strconv.FormatInt(123, 16) // Base 16 (hexadecimal)
	fmt.Println("Hexadecimal String:", strBase)
}
```

## Converting Float to String

For converting a float to a string, use `strconv.FormatFloat`.

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	floatStr := strconv.FormatFloat(123.456, 'f', 2, 64)
	fmt.Println("Formatted Float String:", floatStr)
}
```

## Best Practices

- Use `strconv` functions that match your specific conversion needs for accurate results.
- Handle errors returned by `strconv` functions properly to deal with invalid input.
- Choose appropriate precision and base for formatting functions to balance between readability and data preservation.

## Common Pitfalls

- Failing to handle conversion errors can lead to unexpected behavior. Always check the error returned by `strconv` functions.
- Choosing an inappropriate bit size for conversion functions can lead to incorrect results or overflow, especially with `ParseInt` and `FormatInt`.
- When converting floats to strings, using a hard-coded precision might lead to loss of significant digits.

## Performance Tips

- Be cautious of conversion overhead in performance-critical sections. Batch process if applicable.
- Avoid redundant conversions in loops; cache or compute outside loops when possible.
- Profile your code if you encounter performance bottlenecks due to frequent type conversions.