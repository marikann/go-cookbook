---
title: 'Sorting with sort Package'
description: 'Explore how to sort slices in Go using the slice manipulation capabilities of the sort package.'
date: '2025-03-24'
category: 'Tools'
---

In Go, the `sort` package provides powerful utilities to sort slices, which is a common operation when handling collections of data. This guide covers simple and custom sorting using the `sort` package.

## Basic Sorting with sort.Ints

Sorting a slice of integers can be done easily using `sort.Ints`.

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	nums := []int{3, 1, 4, 1, 5, 9, 2, 6}
	sort.Ints(nums)
	fmt.Println("Sorted integers:", nums)
}
```

## Sorting Strings

When sorting a slice of strings, utilize `sort.Strings`.

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	names := []string{"Charlie", "Alice", "Bob"}
	sort.Strings(names)
	fmt.Println("Sorted strings:", names)
}
```

## Sorting with Custom Criteria

For more complex data types, implement custom sorting by defining the `sort.Interface`.

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
		{Name: "Alice", Age: 25},
		{Name: "Bob", Age: 22},
		{Name: "Charlie", Age: 30},
	}
	sort.Sort(ByAge(people))
	fmt.Println("Sorted by age:", people)
}
```

## Sort.Slice for Interchangeable Less Function

Use `sort.Slice` for inline definition of the `Less` function, which is flexible for one-off sorts.

```go
package main

import (
	"fmt"
	"sort"
)

type Car struct {
	Brand string
	Year  int
}

func main() {
	cars := []Car{
		{Brand: "Toyota", Year: 2010},
		{Brand: "Ford", Year: 2008},
		{Brand: "BMW", Year: 2015},
	}

	sort.Slice(cars, func(i, j int) bool {
		return cars[i].Year < cars[j].Year
	})
	fmt.Println("Cars sorted by year:", cars)
}
```

## Best Practices

- Use `sort.Slice` for ad-hoc and one-time sorting needs instead of implementing full interfaces.
- Implement custom sort interfaces (`sort.Interface`) for sorting complex data types across multiple parts of your codebase.

## Common Pitfalls

- Ensure the `Less` function establishes a strict weak ordering; otherwise, the sort may yield incorrect results.
- Do not forget to swap elements in the `Swap` implementation when defining `sort.Interface`, as improper implementations can lead to unordered slices.

## Performance Tips

- Sort only when necessary; avoid repeated sorts on already sorted data to reduce computational overhead.
- If working on very large datasets, consider using methods that amortize sorting time, like maintaining sorted lists or heaps, rather than sorting slices repeatedly.