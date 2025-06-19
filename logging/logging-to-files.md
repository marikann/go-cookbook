---
title: 'Logging to Files'
description: 'Learn how to log messages to files in Go using standard libraries and best practices'
date: '2025-03-24'
category: 'Logging'
---

Logging is essential for monitoring the behavior of an application in development and production environments. This snippet demonstrates how to perform logging to files in Go using the standard library.

## Basic File Logging

Here's a simple example that shows how to log messages to a file using the built-in `log` package:

```go
package main

import (
	"log"
	"os"
)

func main() {
	// Open file for writing logs.
	f, err := os.OpenFile("app.log", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0666)
	if err != nil {
		log.Fatalf("Error opening file: %v", err)
	}
	defer f.Close()

	// Set output of logs to file.
	log.SetOutput(f)

	// Example log entries.
	log.Println("This is an info message")
	log.Println("This is another log entry")
}
```

## Customizing Logger Output

The `log` package allows you to customize the logger output format:

```go
package main

import (
	"log"
	"os"
)

func main() {
	f, err := os.OpenFile("custom.log", os.O_APPEND|os.O_CREATE|os.O_WRONLY, 0666)
	if err != nil {
		log.Fatalf("Error opening file: %v", err)
	}
	defer f.Close()

	logger := log.New(f, "CUSTOM PREFIX: ", log.Ldate|log.Ltime|log.Lshortfile)
	logger.Println("This is a log entry with a custom prefix and details")
}
```

## Rotating Log Files

For log rotation, consider using popular libraries like [lumberjack](https://github.com/natefinch/lumberjack):

```go
package main

import (
	"log"

	"gopkg.in/natefinch/lumberjack.v2"
)

func main() {
	logger := &lumberjack.Logger{
		Filename:   "rotating.log",
		MaxSize:    5,  // megabytes
		MaxBackups: 3,
		MaxAge:     28, //days
	}
	log.SetOutput(logger)

	// Example log entry.
	log.Println("This is an entry in a rotating log file")
}
```

## Best Practices

- Use structured logging with consistent formats for easier parsing and analysis.
- Implement log rotation using libraries like lumberjack to prevent log files from consuming too much disk space.
- Always use `defer f.Close()` when opening log files to ensure the file is closed when done.

## Common Pitfalls

- Not setting a proper file mode when opening a log file can lead to permission issues.
- Forgetting to set the logger's output to the file can lead to logs being written to stderr instead.
- Ignoring errors from file operations can lead to silent failures in log file writes.
  
## Performance Tips

- Adjust `log.SetFlags` to control the granularity of each log entry for performance tuning.
- Use a buffered writer to improve IO performance if logging is intensive.
- Minimize log entry size during critical operations to reduce latency.