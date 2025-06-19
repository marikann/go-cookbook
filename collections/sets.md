---
title: 'Working with Sets in Go'
description: 'Learn how to create and manipulate sets in Go using basic data structures and packages'
date: '2025-03-24'
category: 'Collections'
---

In Go, there isn't a built-in set data type, but you can implement sets using existing data structures like maps or slices. This snippet demonstrates how to create and manipulate sets effectively.

## Using Maps to Create Sets

Maps in Go can be used to mimic the behavior of sets. Here's how you can implement a simple set using a map with keys as set elements and boolean values as placeholders:

```go
package main

import "fmt"

// StringSet represents a set of strings.
type StringSet map[string]bool

// Add inserts an element into the set.
func (s StringSet) Add(element string) {
	s[element] = true
}

// Remove deletes an element from the set.
func (s StringSet) Remove(element string) {
	delete(s, element)
}

// Contains checks if an element exists in the set.
func (s StringSet) Contains(element string) bool {
	_, exists := s[element]
	return exists
}

func main() {
	set := StringSet{}
	set.Add("golang")
	set.Add("docker")
	set.Add("kubernetes")

	fmt.Println("Set contains 'docker':", set.Contains("docker"))
	set.Remove("docker")
	fmt.Println("Set contains 'docker' after removal:", set.Contains("docker"))
}
```

## Implementing Integer Sets with Slices

For integer sets, you may choose to use slices, providing custom functions for common set operations:

```go
package main

import (
	"fmt"
	"sort"
)

// IntegerSet represents a set of integers.
type IntegerSet []int

// Add inserts an element into the set if it does not already exist.
func (s *IntegerSet) Add(element int) {
	if !s.Contains(element) {
		*s = append(*s, element)
		sort.Ints(*s)
	}
}

// Remove deletes an element from the set.
func (s *IntegerSet) Remove(element int) {
	for i, v := range *s {
		if v == element {
			*s = append((*s)[:i], (*s)[i+1:]...)
			return
		}
	}
}

// Contains checks if an element exists in the set.
func (s IntegerSet) Contains(element int) bool {
	i := sort.SearchInts(s, element)
	return i < len(s) && s[i] == element
}

func main() {
	set := IntegerSet{}
	set.Add(42)
	set.Add(73)
	set.Add(91)

	fmt.Println("Set contains 73:", set.Contains(73))
	set.Remove(73)
	fmt.Println("Set contains 73 after removal:", set.Contains(73))
}
```

## Best Practices

- Use `