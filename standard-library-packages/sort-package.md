---
title: Sort Package
description: Learn how to use Go's sort package to sort slices efficiently.
date: 2025-03-24
category: Standard library packages
difficulty: beginner
---

Go's `sort` package provides functions to sort slices and user-defined collections efficiently. This snippet demonstrates how to use the `sort` package for common sorting tasks.

## Sorting a Slice of Integers

To sort a slice of integers, you can use the `sort.Ints()` function:

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	numbers := []int{4, 2, 3, 1, 5}
	sort.Ints(numbers)
	fmt.Println(numbers) // Output: [1 2 3 4 5]
}
```

## Sorting a Slice of Strings

You can sort a slice of strings using the `sort.Strings()` function:

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	words := []string{"banana", "apple", "cherry"}
	sort.Strings(words)
	fmt.Println(words) // Output: [apple banana cherry]
}
```

## Custom Sorting

To sort a slice with a custom comparator, implement the `sort.Interface` for your slice by defining `Len()`, `Less()`, and `Swap()` methods:

```go
package main

import (
	"fmt"
	"sort"
)

type Person struct {
	Name string
	Age  int
}

type ByAge []Person

func (a ByAge) Len() int           { return len(a) }
func (a ByAge) Swap(i, j int)      { a[i], a[j] = a[j], a[i] }
func (a ByAge) Less(i, j int) bool { return a[i].Age < a[j].Age }

func main() {
	people := []Person{
		{"Alice", 31},
		{"Bob", 25},
		{"Charlie", 29},
	}
	sort.Sort(ByAge(people))
	
	for _, person := range people {
		fmt.Printf("%s: %d\n", person.Name, person.Age)
	}
	// Output: 
	// Bob: 25
	// Charlie: 29
	// Alice: 31
}
```

## Best Practices

- Use the `sort` package functions like `sort.Ints()` and `sort.Strings()` for common cases to simplify your code.
- Leverage `sort.Slice()` for inline custom sorting without the need for a new type, providing greater flexibility without clutter.

## Common Pitfalls

- Forgetting to implement all methods of `sort.Interface` when using custom sort.
- Not understanding that `sort` modifies the original slice and does not return a new sorted slice.
- Using `sort` functions directly on non-slice data types, such as arrays, which will not work.

## Performance Tips

- Use built-in sort functions like `sort.Ints()` for better performance, as they make use of low-level optimizations.
- Avoid redundant sorting operations within a frequently called loop to enhance performance.
- When sorting large slices, consider the overhead and check if sorting fewer elements might suffice for your use case.