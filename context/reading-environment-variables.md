---
title: 'Reading Environment Variables'
description: 'Learn how to read environment variables in Go using the os package'
date: '2025-03-24'
category: 'Context'
---

In Go, environment variables can be accessed using the `os` package. This tutorial shows how to effectively read and use environment variables in your applications.

## Basic Environment Variable Reading

You can use `os.Getenv` to retrieve the value of an environment variable. If the variable is not set, it returns an empty string.

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	path := os.Getenv("PATH")
	fmt.Println("Path:", path)

	home := os.Getenv("HOME")
	fmt.Println("Home:", home)

	// This will print an empty string if not set.
	unset := os.Getenv("NON_EXISTENT_VAR")
	fmt.Println("Unset:", unset)
}
```

## Checking for Environment Variable Existence

To check whether an environment variable is set, compare the result from `os.LookupEnv`. It returns the value and a boolean indicating the presence of the variable.

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	if value, ok := os.LookupEnv("DATA_DIR"); ok {
		fmt.Println("DATA_DIR is set:", value)
	} else {
		fmt.Println("DATA_DIR is not set")
	}
}
```

## Setting Environment Variables

Although not a common practice in production code, Go allows setting environment variables using `os.Setenv`.

```go
package main

import (
	"fmt"
	"os"
)

func main() {
	err := os.Setenv("API_KEY", "73")
	if err != nil {
		fmt.Println("Error:", err)
	}

	value := os.Getenv("API_KEY")
	fmt.Println("API_KEY:", value)
}
```

## Best Practices

- **Use Default Values:** Provide default values if environment variables are missing to ensure your application remains stable.
- **Centralize Access:** Implement a wrapper for accessing environment variables, which can simplify defaults and logging.
- **Security Considerations:** Be cautious with sensitive data stored in environment variables. Avoid logging these values.

## Common Pitfalls

- **Empty Strings:** `Getenv` returns an empty string if the variable is unset, which might mislead if you're checking for empty values.
- **Not Checking Presence:** Failing to confirm if a variable is set can lead to unexpected behavior.
- **Case Sensitivity:** Environment variable names are case-sensitive on Unix but not on Windows, causing potential cross-platform issues.

## Performance Tips

- **Caching Values:** If an environment variable is accessed multiple times, store it in a variable rather than repeatedly calling `os.Getenv`.
- **Minimal Use:** Limit the use of `os.Setenv` in performance-critical paths since it can affect program performance.
- **Concurrent Access:** When accessing environment variables concurrently, ensure thread safety if you choose to modify them during execution.