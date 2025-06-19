---
title: 'String Interpolation in Go'
description: 'Learn how to use string interpolation in Go for dynamic string manipulation using the fmt package.'
date: '2025-03-24'
category: 'Tools'
---

String interpolation in Go is often accomplished using the `fmt` package, which provides powerful string formatting features. This guide will show you how to dynamically construct strings with variables.

## Basic String Interpolation

Here's a simple example illustrating how to interpolate strings using the `fmt.Sprintf` function:

```go
package main

import (
	"fmt"
)

func main() {
	dishName := "Spaghetti Carbonara"
	price := 15.99
	
	// Create a menu item description with price.
	menuItem := fmt.Sprintf("Today's Special: %s - $%.2f", dishName, price)
	fmt.Println(menuItem)  // Output: Today's Special: Spaghetti Carbonara - $15.99
}
```

In this example, `%s` is a placeholder for a string and `%d` is for an integer. These are replaced by the variables `name` and `age` respectively in the output string.

## Interpolation with Multiple Variables

You can interpolate multiple variables and even use different types:

```go
package main

import (
	"fmt"
)

func main() {
	eventName := "Summer Music Festival"
	ticketPrice := 49.99
	availableSeats := 250
	vipAccess := true
	
	// Format event ticket information with multiple variables.
	ticketInfo := fmt.Sprintf("Event: %s\nPrice: $%.2f\nSeats Available: %d\nVIP Access: %t",
		eventName, ticketPrice, availableSeats, vipAccess)
	fmt.Println(ticketInfo)
}
```

## Using Custom Formats

The `fmt` package allows you to define custom formats, which can help in structuring complex strings:

```go
package main

import (
	"fmt"
)

func main() {
	restaurantName := "La Bella Italia"
	rating := 4.8
	cuisine := "Italian"
	
	// Create a formatted restaurant description.
	description := fmt.Sprintf("Welcome to %s\nRating: %.1f/5.0\nCuisine: %s\nReservations Recommended",
		restaurantName, rating, cuisine)
	fmt.Println(description)
}
```

## Best Practices

- Use `fmt.Sprintf` for complex string formatting instead of manual concatenation to improve readability and reduce errors.
- Match format specifiers (`%s`, `%d`, `%f`, etc.) with the corresponding variable types to avoid runtime errors.
- Use named parameters in `Sprintf` function to make your code more readable, especially when dealing with multiple variables.

## Common Pitfalls

- Mismatched format specifiers and variable types can lead to runtime errors or unexpected output. Always check format specifiers carefully.
- Overuse of string interpolation for performance-critical paths can be inefficient as `Sprintf` has overhead compared to simple concatenation.
- Forgetting to handle errors when using `fmt.Fprintf` on files or network connections.

## Performance Tips

- Minimize the use of `Sprintf` within tight loops. Concatenation with `+` can sometimes be more efficient.
- For large strings, use `strings.Builder` to efficiently build strings, reducing unnecessary memory allocations.
- Profile your application's performance if string formatting becomes a bottleneck.

String interpolation with the `fmt` package in Go is a powerful tool for building dynamic strings. By understanding and utilizing format specifiers, you can construct complex strings efficiently and effectively.