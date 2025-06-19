---
title: 'Using Go Reflection'
description: 'Learn how to use reflection in Go to inspect and manipulate types and values'
date: '2025-03-24'
category: 'Reflection'
---

Go's reflection package provides the ability to inspect and manipulate types and values at runtime. This can be useful for writing generic code, or for performing operations that depend on dynamic information about the data.

## Basic Reflection Example

Here's how to inspect the type and value of an interface using reflection in Go:

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var x float64 = 3.14
	t := reflect.TypeOf(x)
	v := reflect.ValueOf(x)

	fmt.Println("Type:", t)
	fmt.Println("Value:", v)
}
```

## Modifying Values

Reflection also allows you to modify the value stored at a given address in memory. To do so, ensure that you pass a pointer to the `reflect.ValueOf` function:

```go
package main

import (
	"fmt"
	"reflect"
)

func main() {
	var x float64 = 1
	v := reflect.ValueOf(&x).Elem() // Get the element that the pointer points to.

	fmt.Println("Original Value:", v)

	v.SetFloat(2) // Modify the value.
	fmt.Println("Modified Value:", v)
	fmt.Println("Underlying value:", x) // x is also changed now.
}
```

## Accessing Struct Fields

You can dynamically access and manipulate struct fields using reflection:

```go
package main

import (
	"fmt"
	"reflect"
)

type Person struct {
	Name string
	Age  int
}

func main() {
	p := Person{"Alice", 25}
	val := reflect.ValueOf(&p).Elem()

	for i := 0; i < val.NumField(); i++ {
		field := val.Field(i)
		fmt.Printf("Field %d: %v\n", i, field)
	}

	// Set field by name.
	if nameField := val.FieldByName("Name"); nameField.IsValid() {
		if nameField.CanSet() {
			nameField.SetString("Bob")
		}
	}

	fmt.Println("Updated Person:", p)
}
```

## Best Practices

- Use reflection sparingly, as it can make code harder to read and maintain.
- When using reflection, always check if the type and value can be modified to avoid run-time panics.
- Ensure that operations are safe; reflection will panic if incorrect methods are used on unsuitable types (for instance, modifying unexported fields).

## Common Pitfalls

- Forgetting to pass a pointer when you want to modify a value using reflection.
- Not accounting for zero values when using `reflect.Value` checks.
- Reflecting on nil interfaces causes panics; always check for nil before reflecting.

## Performance Tips

- Avoid using reflection in performance-critical sections of your code because it is slower than direct operations on types.
- Cache frequently accessed reflection information (like `reflect.Type`) to avoid repetitive expensive operations.
- Profile your use of reflection to understand the performance costs in highly dynamic programs.