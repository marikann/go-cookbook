---
title: 'Getting Command Line Arguments with flag Package'
description: 'Learn how to parse command line arguments in Go using the flag package.'
date: '2025-03-24'
category: 'Tools'
---

Quick recipes for parsing command-line arguments using Go's `flag` package.

## Basic Flag Parsing

```go
package main

import (
	"flag"
	"fmt"
	"time"
)

func main() {
	logFile := flag.String("file", "app.log", "Log file to analyze.")
	since := flag.Duration("since", 24*time.Hour, "Show entries since duration.")
	level := flag.String("level", "ERROR", "Log level to filter (INFO, WARN, ERROR).")
	count := flag.Bool("count", false, "Show entry count only.")

	flag.Parse()

	fmt.Printf("Analyzing %s for %s logs since %v\n", *logFile, *level, *since)
	if *count {
		fmt.Println("Count mode enabled.")
	}
}
```

Run with:
```shell
go run loganalyzer.go -file=server.log -level=ERROR -since=12h -count
```

## Handling Additional Arguments

```go
package main

import (
	"flag"
	"fmt"
)

func main() {
	pattern := flag.String("pattern", ".*", "Regex pattern to match.")
	flag.Parse()

	// Get non-flag arguments (file paths).
	files := flag.Args()
	if len(files) == 0 {
		fmt.Println("Error: No log files specified.")
		fmt.Println("Usage: loggrep -pattern=<regex> file1 [file2 ...]")
		return
	}

	fmt.Printf("Searching for pattern '%s' in:\n", *pattern)
	for i, file := range files {
		fmt.Printf("%d. %s\n", i+1, file)
	}
}
```

Run with:
```shell
go run loggrep.go -pattern="error.*timeout" app.log api.log worker.log
```

## Best Practices

- Ensure you call `flag.Parse()` before accessing any flag values.
- Provide meaningful default values and help messages for each flag.
- Use `flag.Arg(i)` and `flag.NArg()` to access non-flag arguments safely.
- Use only short, fixed-size types (`int32`, `float32`, etc.) if you expect platform portability.

## Common Pitfalls

- Failing to call `flag.Parse()`, which causes the program to not actually parse command line arguments.
- Assuming flag values without verification, leading to unexpected behavior.
- Ignoring default values in flags, which might lead to unexpected behaviors when no value is provided.

## Performance Tips

- Use simple types for flags to minimize parsing overhead.
- Avoid initializing large variables as flags, which can increase memory usage.
- Profile your flag-handling logic if command line parsing becomes a performance bottleneck in resource-constrained environments.