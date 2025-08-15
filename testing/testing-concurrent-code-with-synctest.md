---
title: 'Testing Concurrent Code with testing/synctest'
description: 'Test concurrent Go code with virtualized time using the testing/synctest package'
date: '2025-08-15'
category: 'Testing'
---

Testing concurrent code that involves timing dependencies can be challenging and flaky. The `testing/synctest` package provides a way to test concurrent code with virtualized time, making tests deterministic and fast.

## Basic Synctest Usage

The `testing/synctest` package allows you to test time-dependent concurrent code by controlling the flow of time in your tests.

```go
package main

import (
	"context"
	"testing"
	"testing/synctest"
	"time"
)

// Function that waits for a timeout before returning a result.
func ProcessWithTimeout(ctx context.Context, data string) (string, error) {
	select {
	case <-time.After(5 * time.Second):
		return "processed: " + data, nil
	case <-ctx.Done():
		return "", ctx.Err()
	}
}

func TestProcessWithTimeout(t *testing.T) {
	synctest.Run(t, func() {
		ctx := context.Background()
		
		start := time.Now()
		result, err := ProcessWithTimeout(ctx, "test-data")
		elapsed := time.Since(start)
		
		if err != nil {
			t.Fatalf("unexpected error: %v", err)
		}
		
		if result != "processed: test-data" {
			t.Errorf("expected 'processed: test-data', got %q", result)
		}
		
		// In synctest, time advances instantly
		if elapsed > time.Millisecond {
			t.Errorf("test took too long: %v", elapsed)
		}
	})
}
```

## Testing Concurrent Operations

Test concurrent operations that depend on timing without the unpredictability of real time:

```go
package main

import (
	"context"
	"sync"
	"testing"
	"testing/synctest"
	"time"
)

// Worker that processes items at intervals.
type TimedWorker struct {
	interval time.Duration
	results  []string
	mu       sync.Mutex
}

func (w *TimedWorker) Start(ctx context.Context, items []string) {
	ticker := time.NewTicker(w.interval)
	defer ticker.Stop()
	
	i := 0
	for {
		select {
		case <-ticker.C:
			if i >= len(items) {
				return
			}
			w.mu.Lock()
			w.results = append(w.results, "processed: "+items[i])
			w.mu.Unlock()
			i++
		case <-ctx.Done():
			return
		}
	}
}

func (w *TimedWorker) GetResults() []string {
	w.mu.Lock()
	defer w.mu.Unlock()
	return append([]string(nil), w.results...)
}

func TestTimedWorker(t *testing.T) {
	synctest.Run(t, func() {
		worker := &TimedWorker{interval: time.Second}
		items := []string{"item1", "item2", "item3"}
		
		ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
		defer cancel()
		
		go worker.Start(ctx, items)
		
		// Wait for processing to complete
		time.Sleep(4 * time.Second)
		
		results := worker.GetResults()
		expected := []string{
			"processed: item1",
			"processed: item2", 
			"processed: item3",
		}
		
		if len(results) != len(expected) {
			t.Fatalf("expected %d results, got %d", len(expected), len(results))
		}
		
		for i, result := range results {
			if result != expected[i] {
				t.Errorf("result %d: expected %q, got %q", i, expected[i], result)
			}
		}
	})
}
```

## Testing Race Conditions

Use synctest to expose race conditions that might be timing-dependent:

```go
package main

import (
	"sync"
	"testing"
	"testing/synctest"
	"time"
)

// Counter with a potential race condition.
type Counter struct {
	value int
	mu    sync.Mutex
}

func (c *Counter) Increment() {
	// Simulated work that might expose race conditions.
	time.Sleep(time.Nanosecond)
	c.mu.Lock()
	c.value++
	c.mu.Unlock()
}

func (c *Counter) Value() int {
	c.mu.Lock()
	defer c.mu.Unlock()
	return c.value
}

func TestCounterConcurrency(t *testing.T) {
	synctest.Run(t, func() {
		counter := &Counter{}
		
		var wg sync.WaitGroup
		goroutines := 100
		wg.Add(goroutines)
		
		// Start multiple goroutines incrementing the counter.
		for i := 0; i < goroutines; i++ {
			go func() {
				defer wg.Done()
				counter.Increment()
			}()
		}
		
		wg.Wait()
		
		if counter.Value() != goroutines {
			t.Errorf("expected counter value %d, got %d", goroutines, counter.Value())
		}
	})
}
```

## Best Practices

- Use `synctest.Run` to wrap your concurrent test logic for deterministic time behavior.
- Test timing-dependent code without relying on real wall-clock time.
- Combine with race detection (`go test -race`) for comprehensive concurrent testing.
- Focus on testing the logical correctness of concurrent algorithms rather than performance.

## Common Pitfalls

- Not wrapping test logic with `synctest.Run`, which prevents time virtualization.
- Mixing real time operations with virtualized time in the same test.
- Expecting real performance characteristics in synctest environment.

## Performance Tips

- Synctest makes tests faster by eliminating real time waits.
- Use synctest for testing correctness, not performance characteristics.
- Combine with benchmarks for performance testing of concurrent code.

