---
title: 'Working with Stacks'
description: 'Learn how to implement and use stack data structures in Go'
date: '2025-03-24'
category: 'Collections'
---

Stacks are a fundamental data structure in computer science. They follow a Last-In-First-Out (LIFO) principle, making them ideal for problems requiring reverse data processing. Go, as a language with no built-in stack type, provides flexibility to implement stacks using slices.

## Basic Stack Implementation

Here's a simple stack implementation using Go slices:

```go
package main

import "fmt"

// Stack represents a stack data structure.
type Stack struct {
	items []int
}

// Push adds an item to the stack.
func (s *Stack) Push(item int) {
	s.items = append(s.items, item)
}

// Pop removes and returns the top item from the stack.
func (s *Stack) Pop() (int, bool) {
	if len(s.items) == 0 {
		return 0, false
	}
	item := s.items[len(s.items)-1]
	s.items = s.items[:len(s.items)-1]
	return item, true
}

// IsEmpty checks if the stack is empty.
func (s *Stack) IsEmpty() bool {
	return len(s.items) == 0
}

func main() {
	stack := Stack{}
	stack.Push(42)
	stack.Push(73)
	fmt.Println(stack.Pop()) // Output: 73 true
	fmt.Println(stack.Pop()) // Output: 42 true
	fmt.Println(stack.Pop()) // Output: 0 false
}
```

## Stack with String Items

You can easily modify the stack to work with other data types, such as strings:

```go
package main

import "fmt"

// StringStack represents a stack of strings.
type StringStack struct {
	items []string
}

// Push adds a string to the stack.
func (s *StringStack) Push(item string) {
	s.items = append(s.items, item)
}

// Pop removes and returns the top string from the stack.
func (s *StringStack) Pop() (string, bool) {
	if len(s.items) == 0 {
		return "", false
	}
	item := s.items[len(s.items)-1]
	s.items = s.items[:len(s.items)-1]
	return item, true
}

func main() {
	stack := StringStack{}
	stack.Push("golang")
	stack.Push("docker")
	fmt.Println(stack.Pop()) // Output: docker true
	fmt.Println(stack.Pop()) // Output: golang true
	fmt.Println(stack.Pop()) // Output:  false
}
```

## Best Practices

- Encapsulate stack operations in methods to maintain clear and consistent access patterns.
- Ensure that you check for empty stacks before popping to prevent runtime errors.
- Use Go's `defer` to manage resource cleanup if necessary within complex stack operations.

## Common Pitfalls

- Not checking for an empty stack before pop operations, which can lead to unexpected errors.
- Mismanaging slice capacity by not handling slice reslicing properly, potentially allowing "stale" data leakage.
- Over-reliance on stack when a more suitable data structure might better match the problem domain.

## Performance Tips

- Consider pre-allocating memory for the slice if the number of stack operations is predictable, minimizing reallocation.
- Be mindful of slice copying when the underlying array grows to avoid performance penalties, especially in performance-critical applications.
- Use an interface type for items if versatility is preferred, balancing it against the potential overhead of interface conversions.
