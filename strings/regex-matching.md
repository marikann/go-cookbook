---
title: 'Regex Matching'
description: 'Learn how to perform regex matching in Go using the regexp package'
date: '2025-03-24'
category: 'Strings'
---

Go provides built-in support for regular expressions through the `regexp` package. This snippet demonstrates how to perform regex matching efficiently.

## Basic Regex Matching

Here's a simple example of how to use regex to match patterns in a string:

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	// Create a pattern for matching international phone numbers.
	re := regexp.MustCompile(`\+\d{1,3}[-\s]?\d{1,4}[-\s]?\d{4,10}`)
	text := "Contact numbers: +1-555-1234567, +44 20 7123 4567"
	
	// Find the first valid phone number in the text.
	match := re.FindString(text)
	if match != "" {
		fmt.Printf("Found valid phone number: %s\n", match)  // Output: Found valid phone number: +1-555-1234567
	} else {
		fmt.Println("No valid phone numbers found.")
	}
}
```

## Finding All Matches

You can find all occurrences of a pattern using `FindAllString`:

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	// Create a pattern for matching URLs in text.
	re := regexp.MustCompile(`https?://[\w\-\.]+\.[a-z]{2,}(?:/[^\s]*)?`)
	text := "Links: https://example.com, http://blog.example.com/posts/1, https://api.example.com/v1"
	
	// Find all URLs in the text.
	matches := re.FindAllString(text, -1)
	fmt.Println("Found URLs:", matches)  // Output: Found URLs: [https://example.com http://blog.example.com/posts/1 https://api.example.com/v1]
}
```

## Capturing Groups

Capturing groups allow you to extract specific parts of the matches:

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	// Create a pattern to parse version numbers with components.
	re := regexp.MustCompile(`v(\d+)\.(\d+)(?:\.(\d+))?`)
	text := "Software version: v2.14.3"
	
	// Extract major, minor, and patch versions.
	matches := re.FindStringSubmatch(text)
	if len(matches) > 0 {
		fmt.Printf("Major: %s, Minor: %s, Patch: %s\n", 
			matches[1], matches[2], matches[3])  // Output: Major: 2, Minor: 14, Patch: 3
	}
}
```

## Best Practices

- Use `MustCompile` for a constant expression that should never fail at runtime. Use `Compile` and check for errors if the pattern is dynamic.
- When performing case-insensitive matches, use `(?i)` prefix in patterns or compile patterns with `Flag`.
- For complex expressions, consider testing your regex patterns with online tools before using them in your code.

## Common Pitfalls

- Misunderstanding greedy vs non-greedy matching can lead to unexpected results. Use `*?` for non-greedy quantifiers.
- Forgetting to escape special characters can lead to incorrect patterns.
- Using overly broad patterns can lead to inefficient matching and false positives.

## Performance Tips

- Regex compilation is expensive. Pre-compile your regex patterns if they will be used multiple times.
- Limit the scope of your regex pattern to reduce unnecessary backtracking.
- For simple tasks, consider using string functions (like `strings.Contains` or `strings.Index`) for better performance over regex.