---
title: 'Runtime Statistics'
description: 'Learn how to gather and interpret runtime statistics in Go using the runtime package.'
date: '2025-03-24'
category: 'Runtime'
---

Go provides the `runtime` package, which allows developers to access various statistics about the memory allocator, garbage collector, and more. Understanding these statistics is crucial for optimizing performance and diagnosing issues in Go applications.

## Gathering Basic Memory Statistics

Here's a basic example demonstrating how to collect and print memory statistics using `runtime.MemStats`:

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	var memStats runtime.MemStats

	// Update memStats.
	runtime.ReadMemStats(&memStats)

	// Print some relevant statistics.
	fmt.Printf("Allocated memory: %d bytes\n", memStats.Alloc)
	fmt.Printf("Total allocated memory: %d bytes\n", memStats.TotalAlloc)
	fmt.Printf("Heap memory obtained from system: %d bytes\n", memStats.HeapSys)
	fmt.Printf("Heap objects: %d\n", memStats.HeapObjects)
}
```

## Running Garbage Collector and Inspecting GC Stats

Forcing garbage collection and inspecting garbage collector statistics:

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
	var memStats runtime.MemStats

	// Force garbage collection.
	runtime.GC()

	// Update memStats.
	runtime.ReadMemStats(&memStats)

	// Print garbage collection stats.
	fmt.Printf("Last GC pause: %d ns\n", memStats.PauseNs[(memStats.NumGC+255)%256])
	fmt.Printf("Total GC pauses: %d\n", memStats.PauseTotalNs)
	fmt.Printf("Number of GCs: %d\n", memStats.NumGC)
}
```

## Inspecting Goroutine Statistics

You can also monitor the number of goroutines running in your application:

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

func main() {
	var wg sync.WaitGroup

	// Start some goroutines.
	for i := 0; i < 10; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			// Simulate work.
			runtime.Gosched()
		}()
	}

	fmt.Printf("Number of goroutines: %d\n", runtime.NumGoroutine())

	wg.Wait()

	fmt.Printf("Number of goroutines after wait: %d\n", runtime.NumGoroutine())
}
```

## Best Practices

- Regularly gather runtime statistics during development to identify potential performance bottlenecks.
- Interpret statistics in context with the application’s behavior to avoid premature optimization.
- Use runtime statistics alongside application-specific metrics for a holistic view.
- Monitor runtime statistics in production to catch issues early, leveraging tools like Prometheus for alerting.

## Common Pitfalls

- Misinterpreting statistics such as memory allocations and garbage collection pauses without understanding typical Go memory management behavior.
- Ignoring the impact of runtime.GC() forcing, which can distort metrics if used indiscriminately.
- Over-relying on runtime statistics without considering external factors like system load and network latency.

## Performance Tips

- Use `runtime.GOMAXPROCS()` to configure the maximum number of CPUs, balancing performance and concurrency based on your workload.
- Profile applications regularly using Go's profiling tools (`pprof`) to understand where time and memory are being used.
- Consider adjusting garbage collector parameters using `GOGC` to better fit your application's memory usage patterns.
- For highly concurrent applications, manage goroutine usage meticulously to avoid excessive context switching and memory pressure.