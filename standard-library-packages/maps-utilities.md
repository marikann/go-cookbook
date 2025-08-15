---
title: 'Maps Utilities Package'
description: 'Use the maps package for common map operations like cloning, equality checking, and iteration'
date: '2025-08-15'
category: 'Standard Library Packages'
---

The `maps` package provides utility functions for common map operations, making it easier to work with maps in a functional and efficient manner.

## Basic Map Operations

Use the maps package for fundamental map operations:

```go
package main

import (
	"fmt"
	"maps"
)

func main() {
	original := map[string]int{
		"alice":   25,
		"bob":     30,
		"charlie": 35,
	}
	
	// Clone a map
	clone := maps.Clone(original)
	fmt.Printf("Original: %v\n", original)
	fmt.Printf("Clone: %v\n", clone)
	
	// Modify clone to show they're independent.
	clone["alex"] = 28
	fmt.Printf("After modifying clone:\n")
	fmt.Printf("  Original: %v\n", original)
	fmt.Printf("  Clone: %v\n", clone)
	
	// Check equality.
	other := map[string]int{
		"alice":   25,
		"bob":     30,
		"charlie": 35,
	}
	fmt.Printf("Original equals other: %t\n", maps.Equal(original, other))
	fmt.Printf("Original equals clone: %t\n", maps.Equal(original, clone))
}
```

## Map Copying and Merging

Copy values between maps and merge maps together:

```go
package main

import (
	"fmt"
	"maps"
)

func main() {
	source := map[string]int{
		"a": 1,
		"b": 2,
		"c": 3,
	}
	
	destination := map[string]int{
		"d": 4,
		"e": 5,
	}
	
	fmt.Printf("Before copy:\n")
	fmt.Printf("  Source: %v\n", source)
	fmt.Printf("  Destination: %v\n", destination)
	
	// Copy all entries from source to destination.
	maps.Copy(destination, source)
	fmt.Printf("After copy:\n")
	fmt.Printf("  Destination: %v\n", destination)
	
	// Demonstrate merging with overlapping keys.
	updates := map[string]int{
		"a": 10, // This will overwrite existing value.
		"f": 6,  // This will be added.
	}
	
	maps.Copy(destination, updates)
	fmt.Printf("After merging updates: %v\n", destination)
}
```

## Functional Map Operations

Use maps package functions for functional programming patterns:

```go
package main

import (
	"fmt"
	"maps"
	"slices"
)

func main() {
	scores := map[string]int{
		"math":    95,
		"science": 87,
		"history": 92,
		"art":     78,
		"music":   88,
	}
	
	// Get all keys as a slice.
	subjects := slices.Collect(maps.Keys(scores))
	slices.Sort(subjects) // Sort for consistent output.
	fmt.Printf("Subjects: %v\n", subjects)
	
	// Get all values as a slice.
	allScores := slices.Collect(maps.Values(scores))
	slices.Sort(allScores)
	fmt.Printf("All scores: %v\n", allScores)
	
	// Calculate average using values.
	total := 0
	count := 0
	for score := range maps.Values(scores) {
		total += score
		count++
	}
	average := float64(total) / float64(count)
	fmt.Printf("Average score: %.2f\n", average)
	
	// Find subjects with high scores.
	highScoreSubjects := make([]string, 0)
	for subject, score := range maps.All(scores) {
		if score >= 90 {
			highScoreSubjects = append(highScoreSubjects, subject)
		}
	}
	slices.Sort(highScoreSubjects)
	fmt.Printf("High score subjects (>=90): %v\n", highScoreSubjects)
}
```

## Performance-Optimized Operations

Use maps package functions for efficient bulk operations:

```go
package main

import (
	"fmt"
	"maps"
	"slices"
	"time"
)

func main() {
	// Create a large map for demonstration.
	largeMap := make(map[int]string)
	for i := 0; i < 100000; i++ {
		largeMap[i] = fmt.Sprintf("value_%d", i)
	}
	
	start := time.Now()
	
	// Efficient cloning.
	cloned := maps.Clone(largeMap)
	cloneTime := time.Since(start)
	
	start = time.Now()
	
	// Efficient key collection.
	keys := slices.Collect(maps.Keys(largeMap))
	keyTime := time.Since(start)
	
	start = time.Now()
	
	// Efficient value collection.
	values := slices.Collect(maps.Values(largeMap))
	valueTime := time.Since(start)
	
	fmt.Printf("Performance results for map with %d entries:\n", len(largeMap))
	fmt.Printf("  Clone time: %v\n", cloneTime)
	fmt.Printf("  Key collection time: %v\n", keyTime)
	fmt.Printf("  Value collection time: %v\n", valueTime)
	fmt.Printf("  Collected %d keys and %d values\n", len(keys), len(values))
	fmt.Printf("  Clone has %d entries\n", len(cloned))
	
	// Verify clone independence.
	cloned[999999] = "new_value"
	fmt.Printf("  Original has key 999999: %t\n", maps.Equal(largeMap, cloned) == false)
}
```

## Best Practices

- Use `maps.Clone` instead of manual copying for better performance and correctness.
- Leverage `maps.Keys` and `maps.Values` with iterators for memory-efficient processing.
- Use `maps.Equal` for deep equality comparison instead of manual iteration.
- Combine maps functions with slices utilities for powerful data processing pipelines.

## Common Pitfalls

- Forgetting that `maps.Clone` creates a shallow copy; nested structures are still shared.
- Not considering that `maps.Keys` and `maps.Values` return iterators, not slices directly.
- Assuming that key/value iteration order is consistent between calls.

## Performance Tips

- Maps package functions are optimized for bulk operations and often faster than manual loops.
- Use `maps.Copy` for merging multiple maps efficiently.
- Combine with `slices.Collect` only when you need materialized slices; otherwise use iterators directly.
- Consider the memory implications of cloning large maps vs. selective copying.
