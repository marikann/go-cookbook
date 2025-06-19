---
title: 'Routing HTTP Requests'
description: 'Learn how to route HTTP requests using the Gorilla Mux library in Go.'
date: '2025-03-24'
category: 'HTTP'
---

Routing HTTP requests efficiently is crucial when building web applications. In Go, while you can use the standard `net/http` package, using a third-party library like Gorilla Mux can simplify complex routing scenarios.

## Basic Routing with Gorilla Mux

Gorilla Mux is a popular routing library in Go for its flexibility and features like URL parameters and subrouters.

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter()

	// Define a simple route.
	r.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Welcome to the home page!")
	})

	// Start the server.
	http.ListenAndServe(":8080", r)
}
```

## Handling URL Parameters

Gorilla Mux makes it easy to capture URL parameters and use them in handler functions.

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter()

	// Define a route with a URL parameter.
	r.HandleFunc("/user/{name}", func(w http.ResponseWriter, r *http.Request) {
		vars := mux.Vars(r)
		name := vars["name"]
		fmt.Fprintf(w, "Hello, %s!\n", name)
	})

	// Start the server.
	http.ListenAndServe(":8080", r)
}
```

## Using Middleware with Gorilla Mux

Middleware functions can be used to execute common logic for multiple routes.

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/gorilla/mux"
)

// Logger middleware.
func logger(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		fmt.Println("Request URL:", r.URL)
		next.ServeHTTP(w, r)
	})
}

func main() {
	r := mux.NewRouter()

	// Apply middleware to all routes.
	r.Use(logger)

	r.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "Welcome to the home page with logging!")
	})

	// Start the server.
	http.ListenAndServe(":8080", r)
}
```

## Subrouters for Modular Design

Subrouters can help manage routes by grouping them logically, which is useful for complex applications.

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter()

	// Create a subrouter.
	api := r.PathPrefix("/api").Subrouter()
	api.HandleFunc("/status", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintln(w, "API is running")
	})

	// Start the server.
	http.ListenAndServe(":8080", r)
}
```

## Best Practices

- Employ middleware for logging, authentication, and other cross-cutting concerns.
- Use URL parameters wisely to keep endpoints clean and intuitive.
- Organize routes using subrouters to separate different parts of your API.

## Common Pitfalls

- Forgetting to start the server with `http.ListenAndServe`.
- Not closing response bodies in middleware or handlers if necessary.
- Failing to handle errors when retrieving URL params, leading to runtime panics.
- Misconfiguring paths in complex routes, which can lead to unexpected behavior.

## Performance Tips

- Define routes specifically to avoid unnecessary regular expression parsing.
- Minimize middleware layers in performance-critical paths to reduce request processing time.
- Prioritize routes from most specific to least specific to optimize matching speed.
- Reuse handlers where possible to avoid performance costs associated with repeated object creation.