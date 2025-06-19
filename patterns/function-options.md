---
title: 'Function Options in Go'
description: 'Explore how to use functional options to create flexible and maintainable APIs in Go'
date: '2025-03-24'
category: 'Tools'
---

Functional options in Go provide an idiomatic way to manage the configuration of functions, constructors, and methods, allowing for greater flexibility and readability. This idiom is especially useful when you have a function or constructor with several optional parameters.

## Basic Example of Functional Options

Here's an example that demonstrates how to use functional options to configure a struct:

```go
package main

import (
	"fmt"
)

// Configuration structure.
type Config struct {
	Host    string
	Port    int
	Timeout int
}

// Functional option type.
type Option func(*Config)

// Host sets the Host field in Config.
func Host(host string) Option {
	return func(c *Config) {
		c.Host = host
	}
}

// Port sets the Port field in Config.
func Port(port int) Option {
	return func(c *Config) {
		c.Port = port
	}
}

// Timeout sets the Timeout field in Config.
func Timeout(timeout int) Option {
	return func(c *Config) {
		c.Timeout = timeout
	}
}

// NewConfig initializes a Config with the given options.
func NewConfig(options ...Option) *Config {
	// Default configuration.
	config := &Config{
		Host:    "localhost",
		Port:    8080,
		Timeout: 30,
	}
	for _, option := range options {
		option(config)
	}
	return config
}

func main() {
	config := NewConfig(Host("example.com"), Port(9090))
	fmt.Printf("Config: %+v\n", config)
}
```

## Using Functional Options for APIs

Functional options can make API methods more flexible and maintainable:

```go
package main

import (
	"fmt"
)

type Request struct {
	URL     string
	Method  string
	Headers map[string]string
}

type ReqOption func(*Request)

func WithMethod(method string) ReqOption {
	return func(r *Request) {
		r.Method = method
	}
}

func WithHeader(key, value string) ReqOption {
	return func(r *Request) {
		if r.Headers == nil {
			r.Headers = make(map[string]string)
		}
		r.Headers[key] = value
	}
}

func NewRequest(url string, options ...ReqOption) *Request {
	req := &Request{
		URL:    url,
		Method: "GET", // default method
	}
	for _, opt := range options {
		opt(req)
	}
	return req
}

func main() {
	req := NewRequest("https://api.example.com/v1/resource", WithMethod("POST"), WithHeader("Authorization", "Bearer token"))
	fmt.Printf("Request: %+v\n", req)
}
```

## Best Practices

- Use functional options to provide clear and concise APIs with many optional parameters.
- Set sensible defaults in the constructor for parameters that are not configured via options.
- Document each option clearly to ensure they're easily understandable and maintainable.

## Common Pitfalls

- Not setting defaults can lead to unexpected behavior if the user forgets to configure some fields.
- Overusing functional options might make the API complex, consider the number of options that are genuinely necessary.
- If options interact in complex ways, it could lead to hard-to-debug errors.

## Performance Tips

- Minimize memory allocations within option functions for better performance.
- Avoid deep copying of structs unless necessary, as this impacts performance, especially within high-load scenarios.
- Profile your application when adding numerous functional options to ensure it does not introduce performance bottlenecks.