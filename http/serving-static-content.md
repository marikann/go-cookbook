---
title: 'Serving Static Content'
description: 'Learn how to serve static files using Go with the net/http package'
date: '2025-03-24'
category: 'HTTP'
---

In Go, serving static content like HTML, CSS, JavaScript, and other assets can be efficiently managed using the `net/http` package. This allows for a seamless integration of static file handling within a Go web server.

## Basic Static File Server

Here's a straightforward example of serving static files from a directory:

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	fs := http.FileServer(http.Dir("./static"))
	http.Handle("/", fs)

	log.Println("Serving on :8080...")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```

This will serve files located in the `static` directory of your project and make them accessible through the root URL path.

## Serving from a Subdirectory

To serve files from a subdirectory, wrap the file server with a `http.StripPrefix`:

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	fs := http.FileServer(http.Dir("./static"))
	http.Handle("/static/", http.StripPrefix("/static/", fs))

	log.Println("Serving on :8080...")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		log.Fatal(err)
	}
}
```

This configuration allows files to be accessed via `/static/` in the URL, serving files located in the `./static` directory on disk.

## Best Practices

- Serve static content from a dedicated directory to avoid exposing unintended files.
- Use `http.StripPrefix` judiciously to serve files correctly from nested directories.
- Set proper cache headers for static files to improve client-side performance.
- Secure the server by limiting the file types served, if necessary.

## Common Pitfalls

- Incorrect directory paths can result in `404 Not Found` errors; ensure the file paths specified match the directory layout.
- Failing to use `http.StripPrefix` when serving from a subdirectory can result in mismatch errors.
- Allowing public access to sensitive files by placing them in the same directory as static content.

## Performance Tips

- Enable HTTP compression for text-based files (CSS, JS, HTML) to reduce bandwidth usage.
- Use a Content Delivery Network (CDN) for globally distributing static files.
- Leverage caching at both the server and client to minimize latency and server load.
- Cache large static files aggressively and tune expiration headers based on file volatility.

By effectively serving static content with Go, you can build robust web applications that deliver static assets efficiently and securely.