---
title: 'Working with Unicode Strings'
description: 'Learn how to handle Unicode strings in Go using native string capabilities and standard library functions.'
date: '2025-03-24'
category: 'Tools'
---

Handling Unicode strings in Go is straightforward thanks to Go's native string capabilities and Unicode support in the standard library. This snippet demonstrates how to work with Unicode strings effectively.

## Basic Unicode String Operations

Go strings are UTF-8 encoded by default, allowing you to handle Unicode characters without extra libraries. Here's an example of basic operations on Unicode strings:

```go
package main

import (
	"fmt"
	"unicode/utf8"
)

func main() {
	username := "María 🌟 González"  // A string containing accented characters and emoji.

	// Calculate the length of the username in bytes.
	fmt.Println("Byte length:", len(username))
	
	// Count the actual number of characters (Unicode code points).
	fmt.Println("Character count:", utf8.RuneCountInString(username))
	
	// Display each character and its position in the string.
	for i, r := range username {
		fmt.Printf("Position %d: Character %q (Unicode: %U)\n", i, r, r)
	}
}
```

## Converting Strings to Runes and Back

Sometimes, you might need to work with strings at the rune level, especially if you are processing non-ASCII text:

```go
package main

import (
	"fmt"
)

func main() {
	greeting := "Hello 👋 你好 안녕하세요"  // Multilingual greeting with emoji.
	
	// Convert multilingual greeting to a slice of runes.
	runes := []rune(greeting)
	
	// Create a reversed version of the greeting.
	for i, j := 0, len(runes)-1; i < j; i, j = i+1, j-1 {
		runes[i], runes[j] = runes[j], runes[i]
	}
	
	// Convert the reversed runes back to a string.
	reversedGreeting := string(runes)
	fmt.Println("Original greeting:", greeting)
	fmt.Println("Reversed greeting:", reversedGreeting)
}
```

## Normalizing Unicode Strings

The standard library doesn't directly support Unicode normalization, which can be important for text comparison and searching. However, you can use the `golang.org/x/text/unicode/norm` package for this purpose:

```go
package main

import (
	"fmt"
	"golang.org/x/text/unicode/norm"
)

func main() {
	// Two ways to write the same name: "José" with different Unicode compositions.
	name1 := "José"  // Single character é
	name2 := "Jose\u0301"  // 'e' followed by combining acute accent
	
	// Normalize both versions to the same form.
	normalized1 := norm.NFC.String(name1)
	normalized2 := norm.NFC.String(name2)
	
	// Compare the original and normalized forms.
	fmt.Printf("Original name1: %s (length: %d)\n", name1, len(name1))
	fmt.Printf("Original name2: %s (length: %d)\n", name2, len(name2))
	fmt.Printf("Are normalized versions equal? %v\n", normalized1 == normalized2)
}
```

## Best Practices

- Always use UTF-8 for string manipulation to ensure compatibility and ease of use with Unicode characters.
- When modifying strings, consider converting them to a slice of runes to handle Unicode code points correctly.
- Use the `golang.org/x/text/unicode/norm` package for text normalization when necessary. It's crucial for text comparison and matching.

## Common Pitfalls

- Confusing byte length with character length; always use `utf8.RuneCountInString` for character count.
- Directly accessing indices of strings expecting single-byte characters; use a range loop to safely iterate over runes.
- Forgetting to handle text normalization, leading to unexpected results in text comparisons.

## Performance Tips

- Avoid unnecessary conversions between string and rune slices for simple operations.
- Use slices of bytes or strings directly when working within ASCII or searching a substring.
- Cache results of expensive normalization processes if the same text is processed repeatedly.