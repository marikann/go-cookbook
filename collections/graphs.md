---
title: 'Graph Data Structures in Go'
description: 'Learn how to implement graph data structures in Go using adjacency list and matrix representations'
date: '2025-03-24'
category: 'Collections'
---

Graph data structures are a fundamental concept in computer science, used to model relationships between pairs of objects. In Go, you can implement graphs using different data representations, such as adjacency lists and adjacency matrices. This guide provides examples for these implementations.

## Adjacency List Implementation

The adjacency list is a popular representation of graphs that is efficient in terms of space for sparse graphs. Here's how you can implement an adjacency list in Go:

```go
package main

import "fmt"

type Graph struct {
	vertices map[int][]int
}

func NewGraph() *Graph {
	return &Graph{vertices: make(map[int][]int)}
}

func (g *Graph) AddEdge(src, dest int) {
	g.vertices[src] = append(g.vertices[src], dest)
	g.vertices[dest] = append(g.vertices[dest], src) // For undirected graphs.
}

func (g *Graph) Print() {
	for vertex, edges := range g.vertices {
		fmt.Printf("%d -> %v\n", vertex, edges)
	}
}

func main() {
	graph := NewGraph()
	graph.AddEdge(42, 73)
	graph.AddEdge(42, 91)
	graph.AddEdge(73, 55)
	graph.Print()
}
```

## Adjacency Matrix Implementation

An adjacency matrix is a 2D array where the element at row `i`, column `j` is `1` if there is an edge from vertex `i` to `j`, and `0` otherwise. This method is advantageous for dense graphs:

```go
package main

import "fmt"

type Graph struct {
	matrix [][]int
}

func NewGraph(size int) *Graph {
	matrix := make([][]int, size)
	for i := range matrix {
		matrix[i] = make([]int, size)
	}
	return &Graph{matrix: matrix}
}

func (g *Graph) AddEdge(src, dest int) {
	g.matrix[src][dest] = 1
	g.matrix[dest][src] = 1 // For undirected graphs.
}

func (g *Graph) Print() {
	for _, row := range g.matrix {
		fmt.Println(row)
	}
}

func main() {
	graph := NewGraph(5)
	graph.AddEdge(0, 2)
	graph.AddEdge(0, 4)
	graph.AddEdge(2, 3)
	graph.Print()
}
```

## Best Practices

- Consider the trade-offs between different graph representations: use adjacency lists for sparse graphs and adjacency matrices for dense graphs.
- Clearly define the direction of edges (i.e., whether the graph is directed or undirected) and implement edge addition methods accordingly.
- Consider graph traversal methods (e.g., BFS, DFS) when designing graph APIs to ensure ease of use and extensibility.

## Common Pitfalls

- Failing to account for both directions in undirected graphs by not adding reciprocal edges.
- Using adjacency matrices for very large sparse graphs can consume excessive memory.

## Performance Tips

- Use adjacency lists to reduce space complexity in sparse graphs, minimizing memory overhead.
- Batch graph modifications to minimize the overhead of dynamic memory allocation.
- Pre-calculate potential traversal paths when possible to enhance query performance in read-heavy scenarios.