---
title: 'Working with Arrays'
description: 'Explore how to create and manipulate arrays in Go, including various operations such as accessing elements, iterating, and slicing.'
date: '2025-03-24'
category: 'Collections'
---

Arrays in Go are fixed-size, ordered collections of elements of the same type. This guide covers how to create and work with arrays in Go.

## Declaring and Initializing Arrays

Here's a basic example demonstrating how to declare and initialize arrays in Go:

```go
package main

import "fmt"

func main() {
	var arr1 [5]int                      // Declares an array of 5 integers initialized to zero values.
	arr2 := [5]int{7, 14, 21, 28, 35}    // Declares an array of 5 integers with explicit initialization.
	arr3 := [...]string{"Go", "Rust"}     // Array with inferred size.

	fmt.Println(arr1)
	fmt.Println(arr2)
	fmt.Println(arr3)
}
```

## Accessing Array Elements

Access specific elements within an array by their indices:

```go
package main

import "fmt"

func main() {
	arr := [3]string{"mango", "papaya", "guava"}
	fmt.Println("First element:", arr[0]) // Accessing the first element.
	arr[1] = "lychee"                     // Modifying the second element.
	fmt.Println("Modified array:", arr)
}
```

## Iterating Over Arrays

Use a `for` loop or a `range` loop to iterate over array elements:

```go
package main

import "fmt"

func main() {
	arr := [3]string{"Go", "Java", "Ruby"}

	// Using a for loop.
	for i := 0; i < len(arr); i++ {
		fmt.Println(arr[i])
	}

	// Using a range loop.
	for index, value := range arr {
		fmt.Printf("Index: %d, Value: %s\n", index, value)
	}
}
```

## Copying and Slicing Arrays

While arrays are fixed in size, you can work with slices to manipulate subsets of arrays:

```go
package main

import "fmt"

func main() {
	arr := [5]int{1, 2, 3, 4, 5}

	// Copying an array.
	var copiedArr [5]int
	copy(copiedArr[:], arr[:])  // Convert arrays to slices for copying.
	fmt.Println("Copied array:", copiedArr)

	// Slicing an array.
	slice := arr[1:4]
	fmt.Println("Sliced array:", slice)
}
```

## Best Practices

- Prefer slices over arrays for most use cases, as they offer more flexibility.
- When copying large arrays, consider potential performance impacts due to memory usage.

## Common Pitfalls

- Attempting to resize arrays; remember they have a fixed size, unlike slices.
- Forgetting that the `range` loop by default returns a copy of the element rather than the element itself.
- Misusing index ranges in slices can lead to runtime panics.

## Performance Tips

- If you know the required size, allocating the array beforehand can improve performance by reducing GC overhead.
- Use slices when the size of the data is not known in advance or when resizing is required.
- For read-heavy operations, avoid unnecessary copying by using slicing carefully.