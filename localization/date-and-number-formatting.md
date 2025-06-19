---
title: 'Date and Number Formatting in Go'
description: 'Learn how to format dates and numbers in Go for localization using standard libraries.'
date: '2025-03-24'
category: 'Localization'
---

Proper localization involves displaying dates and numbers in a way that's familiar to users in their region. Go's standard library offers powerful tools for date and number formatting.

## Date Formatting

In Go, date formatting follows a unique pattern using example dates to specify the desired format.

### Basic Date Formatting

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	// Date format in Go is based on Mon Jan 2 15:04:05 MST 2006.
	fmt.Println(t.Format("2006-01-02 15:04:05"))
	fmt.Println(t.Format("02-Jan-2006 3:04PM"))
}
```

### Customizing Date Output

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	t := time.Now()
	// Example to show different date/time formats for localization.
	fmt.Println(t.Format("Monday, 02-Jan-06 15:04:05 MST"))
	fmt.Println(t.Format("02/01/2006 15:04:05"))
}
```

## Number Formatting

Go's `fmt` package provides basic functionality for formatting numbers, but for localization, sometimes additional libraries or manual methods are needed.

### Basic Number Formatting

```go
package main

import (
	"fmt"
)

func main() {
	price := 1234.567
	// Format number with two decimal places.
	fmt.Printf("Price: %.2f\n", price)
}
```

### Number Formatting with `github.com/leekchan/accounting`

For more advanced and localized numbering formats, you might consider third-party libraries such as `accounting`.

```go
package main

import (
	"fmt"
	"github.com/leekchan/accounting"
)

func main() {
	ac := accounting.Accounting{
		Symbol:    "$",
		Precision: 2,
		Thousand:  ",",
		Decimal:   ".",
	}
	
	price := 12345.678
	fmt.Println(ac.FormatMoney(price)) // Outputs: $12,345.68
}
```

## Best Practices

- Use Go's `time.Format` function to handle different date formats. Avoid using string manipulations.
- For basic number formatting, the `fmt` package suffices. For more advanced and localized formatting, explore libraries like `accounting`.
- Consider local cultural conventions when designing date and number formats.

## Common Pitfalls

- Forgetting the unique nature of Go's date formatting based on example dates can lead to errors.
- Overlooking the importance of locale-aware formatting in global applications might result in misunderstandings.

## Performance Tips

- Avoid creating new `time` layouts frequently; reuse them as constants.
- Profile your code to ensure that third-party libraries do not introduce performance bottlenecks, especially in computation-intensive applications.
- Use lazy loading for external libraries in cases where formatting is not frequently needed to optimize runtime performance.