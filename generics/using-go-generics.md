---
title: 'Using Go Generics'
description: 'Explore how to use Go generics to write type-safe, reusable code with examples and best practices.'
date: '2025-03-24'
category: 'Generics'
---

Go 1.18 introduced generics, enabling developers to write type-safe and reusable code using parameterized types. This feature allows functions and data structures to operate with any data type in a type-safe manner.

## Basic Generic Function

Here's a simple example of a generic function that finds the maximum element in a slice of any data type:

```go
package main

import (
	"fmt"
)

type comparable interface {
	~int | ~float64 | ~string
}

func Max[T comparable](slice []T) T {
	var max T
	for i, v := range slice {
		if i == 0 || v > max {
			max = v
		}
	}
	return max
}

func main() {
	ints := []int{1, 2, 3, 4}
	fmt.Println("Max ints:", Max(ints))

	floats := []float64{1.1, 2.2, 3.3, 4.4}
	fmt.Println("Max floats:", Max(floats))

	strings := []string{"apple", "banana", "cherry"}
	fmt.Println("Max strings:", Max(strings))
}
```

## Generic Data Structure

Generics can also be used to create versatile data structures like a stack:

```go
package main

import "fmt"

type Stack[T any] struct {
	items []T
}

func (s *Stack[T]) Push(item T) {
	s.items = append(s.items, item)
}

func (s *Stack[T]) Pop() T {
	if len(s.items) == 0 {
		var zero T
		return zero
	}
	item := s.items[len(s.items)-1]
	s.items = s.items[:len(s.items)-1]
	return item
}

func main() {
	intStack := &Stack[int]{}
	intStack.Push(1)
	intStack.Push(2)
	fmt.Println("Pop:", intStack.Pop())
	fmt.Println("Pop:", intStack.Pop())

	stringStack := &Stack[string]{}
	stringStack.Push("Go")
	stringStack.Push("Generics")
	fmt.Println("Pop:", stringStack.Pop())
}
```

## Best Practices

- Use generics for truly generic operations: Avoid using generics to merely avoid writing specific implementations without clear benefits.
- Leverage type constraints wisely to ensure you only accept types that carry out the operations needed within your generic function or data structure.
- Document your generic functions and types well to clarify both their intent and any unique limitations the generic approach imposes.

## Common Pitfalls

- Overusing generics can cause code complexity. Only use them when type flexibility is necessary.
- Ensure proper type constraints are applied. Inadequate constraints can lead to runtime errors.
- Avoid using generic types in hot paths without considering their potential impact on performance due to type assertions.

## Performance Tips

- Benchmark both generic and non-generic implementations if performance is critical, as the added flexibility might sometimes introduce overhead.
- Use concrete types where possible for performance-critical sections of the code to avoid dynamic type dispatch.
- Lean on compiler optimizations and testing to determine any unforeseen inefficiencies with the chosen generic algorithms.