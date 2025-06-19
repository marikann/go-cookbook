---
title: 'Generic Data Structures in Go'
description: 'Explore how to implement generic data structures using Go generics, introduced in Go 1.18.'
date: '2025-03-24'
category: 'Generics'
---

Go 1.18 introduced support for type parameters, commonly known as generics, which allow you to write more reusable and adaptable data structures. In this snippet, we'll explore how to create basic generic data structures.

## Generic Stack Implementation

A stack is a classic Last-In-First-Out (LIFO) data structure. Here's how you can implement a generic stack in Go:

```go
package main

import "fmt"

type Stack[T any] struct {
	data []T
}

// Push adds an element to the stack.
func (s *Stack[T]) Push(v T) {
	s.data = append(s.data, v)
}

// Pop removes and returns the last element of the stack.
func (s *Stack[T]) Pop() (T, bool) {
	if len(s.data) == 0 {
		var zeroValue T
		return zeroValue, false
	}
	v := s.data[len(s.data)-1]
	s.data = s.data[:len(s.data)-1]
	return v, true
}

func main() {
	s := Stack[int]{}
	s.Push(1)
	s.Push(2)
	fmt.Println(s.Pop()) // Output: 2, true
	fmt.Println(s.Pop()) // Output: 1, true
	fmt.Println(s.Pop()) // Output: 0, false
}
```

## Generic Queue Implementation

A queue is a First-In-First-Out (FIFO) data structure. Here's an example implementation:

```go
package main

import "fmt"

type Queue[T any] struct {
	data []T
}

// Enqueue adds an element to the queue.
func (q *Queue[T]) Enqueue(v T) {
	q.data = append(q.data, v)
}

// Dequeue removes and returns the first element from the queue.
func (q *Queue[T]) Dequeue() (T, bool) {
	if len(q.data) == 0 {
		var zeroValue T
		return zeroValue, false
	}
	v := q.data[0]
	q.data = q.data[1:]
	return v, true
}

func main() {
	q := Queue[string]{}
	q.Enqueue("hello")
	q.Enqueue("world")
	fmt.Println(q.Dequeue()) // Output: hello, true
	fmt.Println(q.Dequeue()) // Output: world, true
	fmt.Println(q.Dequeue()) // Output: , false
}
```

## Best Practices

- Ensure type safety by leveraging Go's type parameters (`T any`) which restrict what types can be used.

## Common Pitfalls

- Avoid using interface{} for handling generic types; explicit type parameters are more efficient and type-safe.
- Be cautious of copying large data when implementing operations such as Push/Pop on slices.
- Ensure proper handling of zero values as Go generics require returning the zero value for the type parameter.

## Performance Tips

- Pre-allocate the underlying slice when the size of the stack or queue can be estimated in advance to reduce allocations.
- For concurrent use, consider wrapping your data structures with synchronization mechanisms like channels or mutexes.
- Profile your code to understand the impact of different type choices and operations on performance.