---
title: 'String Manipulation in Go'
description: 'Learn how to manipulate strings in Go using various built-in functions and packages'
date: '2025-03-24'
category: 'Tools'
---

Go provides a vast array of built-in functionality for string manipulation. The `strings` package provides many utility functions, while basic operations can be performed using standard operators.

## Basic String Manipulation

Here's how you can perform some simple string manipulations:

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	customerName := "John Smith"
	
	// Convert customer name to uppercase for display formatting.
	upper := strings.ToUpper(customerName)
	fmt.Println(upper)  // Output: JOHN SMITH
	
	// Convert customer name to lowercase for case-insensitive comparison.
	lower := strings.ToLower(customerName)
	fmt.Println(lower)  // Output: john smith
	
	// Check if customer name contains "Smith".
	fmt.Println(strings.Contains(customerName, "Smith"))  // Output: true
	
	// Split customer name into first and last name.
	nameParts := strings.Split(customerName, " ")
	fmt.Println(nameParts)  // Output: [John Smith]
	
	// Join customer details with commas.
	customerInfo := strings.Join([]string{"John", "Smith", "Premium"}, ", ")
	fmt.Println(customerInfo)  // Output: John, Smith, Premium
}
```

## Trimming and Replacing Strings

Go provides concise ways to trim and replace parts of strings:

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	userInput := "  support@example.com  "

	// Remove leading and trailing whitespace from email address.
	trimmedEmail := strings.TrimSpace(userInput)
	fmt.Println(trimmedEmail)  // Output: support@example.com
	
	// Remove specific characters from the input.
	domainOnly := strings.Trim(trimmedEmail, "support@")
	fmt.Println(domainOnly)  // Output: example.com
	
	// Replace domain for email migration.
	newEmail := strings.Replace(trimmedEmail, "example.com", "newdomain.com", 1)
	fmt.Println(newEmail)  // Output: support@newdomain.com
}
```

## String Concatenation

In Go, strings can be concatenated using the `+` operator or the `strings.Builder` for more efficient concatenation:

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	firstName := "Sarah"
	lastName := "Johnson"

	// Simple concatenation for full name.
	fullName := firstName + " " + lastName
	fmt.Println(fullName)  // Output: Sarah Johnson

	// Using strings.Builder for building a customer greeting.
	var builder strings.Builder
	builder.WriteString("Dear ")
	builder.WriteString(firstName)
	builder.WriteString(" ")
	builder.WriteString(lastName)
	builder.WriteString(", welcome!")
	fmt.Println(builder.String())  // Output: Dear Sarah Johnson, welcome!
}
```

## Best Practices

- Use `strings.Builder` for efficient concatenation in loops or when performance is critical.
- Leverage `strings` package functions for common operations, as they are optimized and concise.
- Use the `+` operator for simple and small-scale concatenations for better readability.

## Common Pitfalls

- Avoid concatenating large strings with the `+` operator inside loops due to potential performance degradation.
- Don't forget about Unicode support when performing operations such as slicing strings; use `[]rune` for safer operations when needed.

## Performance Tips

- Use `strings.Builder` for building strings in performance-sensitive code to minimize memory allocations.
- Pre-allocate space where possible, particularly when iteratively building strings, for optimal memory usage.
- Profile your code when processing extensive strings to ensure that the chosen method meets the performance criteria.