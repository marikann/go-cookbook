---
title: 'String Conversion in Go'
description: 'Learn how to perform string conversions in Go, including basic and advanced use cases.'
date: '2025-03-24'
category: 'Tools'
---

String conversion is a common task in programming that involves converting one data type to a string or vice versa. Go, a statically typed language, provides robust support for string conversions through its standard library, particularly the `strconv` package.

## Basic String Conversion

### Converting Integers to Strings

To convert an integer to a string, use the `strconv.Itoa` function:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	productID := 7842
	// Convert product ID to string for display formatting.
	strID := strconv.Itoa(productID)
	fmt.Printf("Product SKU: SKU-%s\n", strID)  // Output: "Product SKU: SKU-7842"
}
```

### Converting Strings to Integers

Use `strconv.Atoi` to convert a string to an integer:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	quantityStr := "150"
	// Parse inventory quantity from string input.
	quantity, err := strconv.Atoi(quantityStr)
	if err != nil {
		fmt.Println("Error: Invalid quantity format.")
		return
	}
	fmt.Printf("Current stock level: %d units\n", quantity)  // Output: Current stock level: 150 units
}
```

## Advanced String Conversion

### Formatting Numbers

For more controlled conversions, such as binary, octal, or hexadecimal formats, use `strconv.FormatInt`:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	sensorValue := int64(1023)
	// Convert sensor reading to different formats for debugging.
	binaryReading := strconv.FormatInt(sensorValue, 2)
	hexReading := strconv.FormatInt(sensorValue, 16)

	fmt.Printf("Sensor reading (binary): %s\n", binaryReading)    // Output: "1111111111"
	fmt.Printf("Sensor reading (hex): 0x%s\n", hexReading)        // Output: "0x3ff"
}
```

### Parsing Floats

To convert a string to a floating-point number, use `strconv.ParseFloat`:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	temperatureStr := "23.45"
	// Parse temperature reading from sensor data string.
	temperature, err := strconv.ParseFloat(temperatureStr, 64)
	if err != nil {
		fmt.Println("Error: Invalid temperature reading.")
		return
	}
	fmt.Printf("Current temperature: %.2f°C\n", temperature)  // Output: Current temperature: 23.45°C
}
```

## Best Practices

- Always handle errors returned by conversion functions like `strconv.Atoi` and `strconv.ParseFloat` to avoid runtime panics.
- Prefer `strconv` over `fmt.Sprintf` for critical performance sections because `fmt.Sprintf` is generally slower.
- When converting large arrays or slices of numbers, consider using loops with preallocated slices to enhance performance.

## Common Pitfalls

- Not checking for conversion errors can lead to unexpected runtime behaviors.
- Using the wrong conversion function (e.g., using `strconv.Atoi` on a non-numeric string) will always result in an error.
- Forgetting that `strconv.ParseFloat` requires you to specify the bit size (32 or 64) can lead to inaccurate results.

## Performance Tips

- Precompute required buffer sizes for conversions when working with multiple number-string conversions in performance-critical settings.
- If precision is not a key concern in floating-point to string conversions, consider limiting decimal places to reduce processing time.
- Profile your conversion-heavy code using Go's `pprof` tool to ensure optimal performance.