---
title: 'Setting Request Timeouts in HTTP'
description: 'Learn how to set request timeouts for HTTP clients in Go, ensuring that your application handles network delays gracefully.'
date: '2025-03-24'
category: 'HTTP'
---

In Go, you can control the maximum amount of time to wait for an HTTP request through timeouts. This can prevent your application from hanging indefinitely on slow or unresponsive networks. Here's how to set up and manage request timeouts using the built-in `net/http` package.

## Basic Timeout Setup

The `http.Client` struct provides options for setting timeouts. Here's a basic example of how to configure timeouts for an HTTP client:

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

func main() {
	client := &http.Client{
		Timeout: 10 * time.Second, // Set a timeout for the entire request
	}

	resp, err := client.Get("https://api.example.com/data")
	if err != nil {
		fmt.Println("Request failed:", err)
		return
	}
	defer resp.Body.Close()

	fmt.Println("Response status:", resp.Status)
}
```

## Customizing Timeouts with `http.Transport`

For more granular control over different phases of the request lifecycle, you can configure a custom `http.Transport`:

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

func main() {
	transport := &http.Transport{
		ResponseHeaderTimeout: 5 * time.Second, // Time to wait for server's first response header
		ExpectContinueTimeout: 1 * time.Second, // Time to wait for a response after sending an `Expect: 100-continue` header
	}

	client := &http.Client{
		Transport: transport,
		Timeout:   15 * time.Second, // Still advisable to set an overall timeout
	}

	resp, err := client.Get("https://api.example.com/data")
	if err != nil {
		fmt.Println("Request failed:", err)
		return
	}
	defer resp.Body.Close()

	fmt.Println("Response status:", resp.Status)
}
```

## Best Practices

- **Set a Reasonable Timeout:** Consider setting a timeout that balances responsiveness with allowing enough time for slow network conditions.
- **Specific Transport Configuration:** Use the `http.Transport` settings to control specific phases (e.g., Dial, TLS handshake).
- **Use Contexts for Server Request Timeouts:** When writing an HTTP server, use contexts to manage and propagate timeouts and cancellations.

## Common Pitfalls

- **Omitting Timeout:** Failing to set a timeout may lead to indefinite hangs, especially on unreliable networks.
- **Overly Short Timeouts:** Extremely short timeouts can lead to frequent errors in less-than-ideal network conditions.
- **Only Using `Timeout` Field:** Relying solely on the `Timeout` field doesn't allow for fine-tuning different stages of the request process.

## Performance Tips

- **Reuse Transports:** Creating new `Transport` instances for every request increases overhead. Reuse existing ones whenever possible.
- **Profile Network Boundaries:** Timeouts should be set based on profiling and understanding the typical network latency and reliability.
- **Concurrent Requests:** Consider rate limiting when making many requests to the same server to prevent exhausting server resources.