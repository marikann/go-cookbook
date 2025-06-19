---
title: 'Priority Queues'
description: 'Implement priority queues in Go using the container/heap package.'
date: '2025-03-24'
category: 'Collections'
---

Priority queues allow for efficient retrieval of the highest (or lowest) priority element. Go doesn't have a built-in priority queue, but you can implement one using the `container/heap` package. In this guide, you'll learn how to implement a priority queue in Go from scratch.

## Basic Priority Queue Implementation

A priority queue can be represented as a heap. The `container/heap` package provides heap operations, which we can leverage to create a priority queue.

```go
package main

import (
	"container/heap"
	"fmt"
)

// An Item is something we manage in a priority queue.
type Item struct {
	value    string // The value of the item; arbitrary.
	priority int    // The priority of the item.
	index    int    // The index of the item in the heap.
}

// A PriorityQueue implements heap.Interface and holds Items.
type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }

func (pq PriorityQueue) Less(i, j int) bool {
	// We want Pop to give us the highest priority so we use greater than here.
	return pq[i].priority > pq[j].priority
}

func (pq PriorityQueue) Swap(i, j int) {
	pq[i], pq[j] = pq[j], pq[i]
	pq[i].index = i
	pq[j].index = j
}

// Push and Pop use pointer receivers because they modify the slice's length,
// not just its contents.
func (pq *PriorityQueue) Push(x interface{}) {
	n := len(*pq)
	item := x.(*Item)
	item.index = n
	*pq = append(*pq, item)
}

func (pq *PriorityQueue) Pop() interface{} {
	old := *pq
	n := len(old)
	item := old[n-1]
	item.index = -1 // for safety
	*pq = old[0 : n-1]
	return item
}

func main() {
	// Create a priority queue and add some items.
	items := map[string]int{
		"task1": 40, "task2": 20, "task3": 70,
	}

	pq := make(PriorityQueue, len(items))
	i := 0
	for value, priority := range items {
		pq[i] = &Item{
			value:    value,
			priority: priority,
			index:    i,
		}
		i++
	}
	heap.Init(&pq)

	// Insert a new item and change the priority of an existing item.
	item := &Item{
		value:    "task4",
		priority: 50,
	}
	heap.Push(&pq, item)
	pq.update(item, item.value, 95)

	// Take the items out; they arrive in decreasing priority.
	for pq.Len() > 0 {
		item := heap.Pop(&pq).(*Item)
		fmt.Printf("%.2d:%s ", item.priority, item.value)
	}
}

func (pq *PriorityQueue) update(item *Item, value string, priority int) {
	item.value = value
	item.priority = priority
	heap.Fix(pq, item.index)
}
```

## Best Practices

- **Use `container/heap` package**: Leverage Go's `container/heap` package for managing the heap operations in a priority queue, as it ensures the proper operation and error-free implementation of heap operations.
  
- **Custom Priority Logic**: Define clear and consistent priority logic in the `Less` method to match the specific requirements for ordering items.

## Common Pitfalls

- **Ignoring the Index Management**: Ensure that the index of each item is updated during `Swap` operations; otherwise, the heap may become invalidated.

- **Priority Changes**: Failing to update the heap structure properly when item priorities change by not using `heap.Fix` may result in incorrect behavior.

- **Performance Overhead**: Using the heap interface inefficiently can lead to performance bottlenecks, particularly in high-frequency operations.

## Performance Tips

- **Profiling**: Profile the priority queue implementation to spot inefficient operations, especially when handling large data sets or frequent updates.

- **Batch Processing**: If possible, handle operations in batches to minimize the heap operations overhead, such as bulk inserting or removing items.

- **Minimize Updates**: Minimize priority updates as these can require a reorganization of the entire heap, impacting performance negatively. Instead, consider designs that minimize such updates.