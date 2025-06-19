---
title: 'Understanding Type Parameters in Go'
description: 'Learn about type parameters in Go and how to use them effectively in generic programming.'
date: '2025-03-24'
category: 'Language idioms'
---

Go's introduction of generic programming brings type parameters, a powerful addition that enhances code reusability and type safety. This guide will walk you through the basics of using type parameters in Go.

## Basic Usage of Type Parameters

Here's a fundamental example of a generic function that accepts a slice of any type and returns its length:

```go
package main

import "fmt"

// Length function using type parameters.
func Length[T any](s []T) int {
	return len(s)
}

func main() {
	ints := []int{1, 2, 3, 4}
	strs := []string{"a", "b", "c"}

	fmt.Println("Length of ints:", Length(ints))
	fmt.Println("Length of strs:", Length(strs))
}
```

## Using Type Parameters with Constraints

Type parameters can have constraints to enforce certain operations or methods. Let's see how to use constraints with a custom constraint:

```go
package main

import (
	"fmt"
	"golang.org/x/exp/constraints"
)

// Sum function with a constraint on numeric types.
func Sum[T constraints.Ordered](s []T) T {
	var total T
	for _, v := range s {
		total += v
	}
	return total
}

func main() {
	ints := []int{1, 2, 3, 4}
	floats := []float64{1.1, 2.2, 3.3}

	fmt.Println("Sum of integers:", Sum(ints))
	fmt.Println("Sum of floats:", Sum(floats))
}
```

## Type Parameters in Structs

Type parameters can also be used in struct definitions to create generic types:

```go
package main

import "fmt"

// Pair is a generic struct.
type Pair[T any, U any] struct {
	First  T
	Second U
}

func main() {
	intStr := Pair[int, string]{1, "one"}
	fmt.Printf("Pair: %v\n", intStr)

	strStr := Pair[string, string]{"hello", "world"}
	fmt.Printf("Pair: %v\n", strStr)
}
```

## Best Practices

- Use type parameters to improve code reusability and maintainability.
- Apply constraints to ensure that only types capable of certain operations are allowed.
- Carefully select names for type parameters and constraints to enhance code readability.
  
## Common Pitfalls

- Overusing type parameters can lead to unnecessary complexity. Use them where they provide clear benefits.
- Forgetting to apply constraints can lead to issues when specific type capabilities are assumed.

## Performance Tips

- Use type constraints to ensure only types with efficient operations are used.
- Avoid unnecessary conversions by leveraging type parameters directly in calculations and operations.
- Profile type-parametrized code to understand its impact on performance, especially in computation-heavy use cases.

Mastering type parameters can significantly enhance your Go programming skills, allowing you to write more flexible and efficient code.