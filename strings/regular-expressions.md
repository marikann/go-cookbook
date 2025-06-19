---
title: 'Regular Expressions in Go'
description: 'Explore how to work with regular expressions in Go using the regexp package.'
date: '2025-03-24'
category: 'Tools'
---

Regular expressions are a powerful tool for matching and manipulating strings. Go offers built-in support for regular expressions via the `regexp` package. This guide will help you understand and utilize regular expressions effectively in your Go programs.

## Simple Regular Expression Matching

Here's a basic example demonstrating how to check if a string matches a regular expression:

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	// Check if the email address starts with a valid character pattern.
	match, _ := regexp.MatchString(`^[a-zA-Z0-9._%+-]+@`, "user.name@example.com")
	fmt.Println("Valid email prefix:", match)  // Output: true
}
```

## Compiling Regular Expressions

For more efficient repeated matching, compile your regular expressions using `regexp.Compile`:

```go
package main

import (
	"fmt"
	"log"
	"regexp"
)

func main() {
	// Compile a regex pattern for matching email domains.
	re, err := regexp.Compile(`@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$`)
	if err != nil {
		log.Fatal("Invalid regex pattern.")
	}

	// Check if email domain is valid.
	email := "contact@company.com"
	fmt.Printf("Email domain is valid: %v\n", re.MatchString(email))  // Output: true
}
```

## Extracting Submatches

You can extract submatches (capture groups) from strings that match your regular expression:

```go
package main

import (
	"fmt"
	"log"
	"regexp"
)

func main() {
	// Create a pattern to extract timestamp and log level from log entries.
	re, err := regexp.Compile(`(\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) \[([A-Z]+)\]`)
	if err != nil {
		log.Fatal("Invalid regex pattern.")
	}

	logLine := "2024-03-24 15:30:45 [ERROR] Failed to connect to database"
	match := re.FindStringSubmatch(logLine)
	if match != nil {
		fmt.Printf("Timestamp: %s, Log Level: %s\n", match[1], match[2])  // Output: Timestamp: 2024-03-24 15:30:45, Log Level: ERROR
	}
}
```

## Replacing Patterns in Strings

Use `regexp.ReplaceAllString` to replace parts of a string that match a pattern:

```go
package main

import (
	"fmt"
	"log"
	"regexp"
)

func main() {
	// Create a pattern to mask sensitive information in logs.
	re, err := regexp.Compile(`password=\w+`)
	if err != nil {
		log.Fatal("Invalid regex pattern.")
	}

	// Replace password with masked value.
	logEntry := "User login attempt with password=secret123 from IP=192.168.1.1"
	maskedLog := re.ReplaceAllString(logEntry, "password=*****")
	fmt.Println("Masked log:", maskedLog)  // Output: User login attempt with password=***** from IP=192.168.1.1
}
```

## Best Practices

- **Compile Once:** Reuse compiled regexp patterns for better performance when you use them multiple times.
- **Test Patterns:** Always test your regular expressions thoroughly to ensure they work as expected.
- **Use Verbose Mode:** For complex patterns, consider using the verbose mode (`(?x)`) for readability.
- **Error Handling:** Always check for errors when compiling regular expressions, especially when dealing with user inputs.

## Common Pitfalls

- **Ignoring Errors:** Do not ignore errors from `regexp.Compile`, as they may lead to runtime panics.
- **Overusing Regular Expressions:** Avoid using regular expressions when simpler string operations would suffice for performance reasons.
- **Complex Patterns:** Be cautious with overly complex patterns, as they can be hard to maintain and debug.

## Performance Tips

- **Precompilation:** Precompile regular expressions if they're used in loops or frequently called paths.
- **Regexp Efficiency:** Make sure your regular expressions do not contain unnecessary patterns or backtracking, which can degrade performance.
- **Avoid Unnecessary Capturing Groups:** Use non-capturing groups `(?:...)` when you don't need the content of a group, to improve speed.

By understanding and applying these techniques, you can effectively handle string matching, searching, and transformations in your Go applications using regular expressions.