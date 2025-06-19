---
title: 'Distributed Locks'
description: 'Learn how to implement distributed locks in Go using various techniques.'
date: '2025-03-24'
category: 'Distributed Systems'
---

Distributed locks are crucial in distributed systems to ensure that multiple nodes don't concurrently modify shared resources. This snippet demonstrates how to implement distributed locks in Go using different techniques.

## Basic Distributed Lock with Redis

Redis is a popular choice for implementing distributed locks due to its speed and simplicity. Below is an example using the `go-redis` library:

```go
package main

import (
	"context"
	"fmt"
	"time"
	
	"github.com/go-redis/redis/v8"
)

var ctx = context.Background()

func main() {
	client := redis.NewClient(&redis.Options{
		Addr: "localhost:6379",
	})

	lockKey := "my-lock"
	expiration := 10 * time.Second

	set, err := client.SetNX(ctx, lockKey, "unique-value", expiration).Result()
	if err != nil {
		panic(err)
	}

	if set {
		fmt.Println("Lock acquired!")
		// Perform your critical section work here.
		time.Sleep(8 * time.Second) // Simulate work

		// Release the lock.
		client.Del(ctx, lockKey)
		fmt.Println("Lock released!")
	} else {
		fmt.Println("Could not acquire lock!")
	}
}
```

## Distributed Lock with Etcd

Etcd provides strong consistency and is another option for implementing distributed locks. Below is an implementation using the `go.etcd.io/etcd/clientv3` package:

```go
package main

import (
	"context"
	"fmt"
	"time"

	clientv3 "go.etcd.io/etcd/client/v3"
	"go.etcd.io/etcd/client/v3/concurrency"
)

func main() {
	client, err := clientv3.New(clientv3.Config{
		Endpoints:   []string{"localhost:2379"},
		DialTimeout: 5 * time.Second,
	})
	if err != nil {
		panic(err)
	}
	defer client.Close()

	session, err := concurrency.NewSession(client)
	if err != nil {
		panic(err)
	}
	defer session.Close()

	mutex := concurrency.NewMutex(session, "/my-lock/")
	if err := mutex.Lock(context.Background()); err != nil {
		panic(err)
	}

	fmt.Println("Lock acquired!")
	// Perform your critical section work here.
	time.Sleep(8 * time.Second) // Simulate work

	if err := mutex.Unlock(context.Background()); err != nil {
		panic(err)
	}
	fmt.Println("Lock released!")
}
```

## Best Practices

- Always set a reasonable expiration time to avoid deadlocks if the process holding the lock crashes.
- Use unique identifiers, such as UUIDs, for each lock value to ensure proper lock release.
- Ensure the distributed locking system is highly available to prevent a single point of failure.
- Consider using TTL (Time To Live) to automatically release locks that have been held too long.

## Common Pitfalls

- Forgetting to release the lock can lead to resource starvation. Ensure you release the lock in a `defer` statement whenever possible.
- Relying on a single point of failure for the lock service can lead to system outage. Always configure redundancy and backups.
- Improper handling of lock expiration can lead to race conditions. Consider implementing a heart-beat mechanism for extending lock durations safely.

## Performance Tips

- Use a dedicated, fast, and consistent data store for managing your distributed locks.
- Consider lock contention strategies to reduce the blocking time for nodes trying to acquire the lock.
- Optimize network latency between distributed nodes and the lock management service for faster lock acquisition and release.
- Monitor and log lock usage patterns to identify and optimize bottlenecks in your system.