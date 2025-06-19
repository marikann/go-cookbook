---
title: 'Container Package'
description: 'Explore the container package in Go, which provides implementations of fundamental data structures like heaps, linked lists, and rings.'
date: '2025-03-24'
category: 'Standard Library Packages'
---

Go's `container` package provides efficient implementations of common data structures including heaps, linked lists, and circular lists (rings). This guide will demonstrate how to use these fundamental structures with concise examples.

## Using container/heap

The `heap` package provides an implementation of a min-heap. You'll need to implement the `heap.Interface` to use it with your custom types.

```go
package main

import (
	"container/heap"
	"fmt"
)

// An IntHeap is a min-heap of ints.
type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x any) {
	*h = append(*h, x.(int))
}

func (h *IntHeap) Pop() any {
	old := *h
	n := len(old)
	x := old[n-1]
	*h = old[0 : n-1]
	return x
}

func main() {
	h := &IntHeap{2, 1, 5}
	heap.Init(h)
	heap.Push(h, 3)
	fmt.Printf("Minimum: %d\n", (*h)[0])

	for h.Len() > 0 {
		fmt.Printf("%d ", heap.Pop(h))
	}
}
```

## Using container/list

The `list` package provides doubly linked lists which are useful for element insertion and deletion that's performance-critical.

```go
package main

import (
	"container/list"
	"fmt"
)

func main() {
	l := list.New()

	e1 := l.PushBack(1)
	e2 := l.PushBack(2)
	l.InsertAfter(3, e1)

	for e := l.Front(); e != nil; e = e.Next() {
		fmt.Println(e.Value)
	}

	l.Remove(e2)
	fmt.Println("After removal:")
	for e := l.Front(); e != nil; e = e.Next() {
		fmt.Println(e.Value)
	}
}
```

## Using container/ring

The `ring` package provides circular, or ring buffers which continue from the beginning once reaching the end.

```go
package main

import (
	"container/ring"
	"fmt"
)

func main() {
	r := ring.New(3)

	for i := 1; i <= r.Len(); i++ {
		r.Value = i
		r = r.Next()
	}

	r.Do(func(value any) {
		fmt.Println(value)
	})
}
```

## Best Practices

- When using `container/heap`, ensure the methods of `heap.Interface` are correctly implemented as incorrect definitions can lead to unexpected behavior.
- Use `container/list` when you need quick insertions and deletions from any part of a sequence, as long as keeping wider access times in check.
- Remember that `container/ring` will overwrite previous entries when it circles back. Be mindful of this to prevent data loss.

## Common Pitfalls

- Failing to initialize a list or ring before using it can result in nil reference errors.
- Modifying the `heap` data manually without using `Push`, `Pop`, or adjusting by other means will disrupt heap property.
- Over-relying on `container/list` for operations that would be better suited with slices or other data structures due to the need for consistent random access patterns.

## Performance Tips

- For optimal performance with heaps, always ensure that `Push` and `Pop` are used for maintaining proper ordering.
- Although `container/list` is more flexible with operations like insertion and deletion, it is less cache-friendly than slices and can have lower performance for operations that are slice-friendly.
- `container/ring` allows efficient fixed-size circular buffer implementations without shifting elements around, offering consistent time complexity for a fixed-length circular traversal.