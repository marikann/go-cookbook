---
title: 'Using the bufio Package for I/O Operations'
description: 'Learn how to utilize the bufio package to optimize I/O operations in Go applications.'
date: '2025-03-24'
category: 'Standard library packages'
---

The `bufio` package in Go provides buffered I/O operations which can help optimize reading and writing of data by reducing the number of system calls. This is particularly beneficial when dealing with large files or network connections.

## Buffered Reading

Here's how you can use `bufio` to read data efficiently from a file:

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	// Open a file for reading.
	file, err := os.Open("example.txt")
	if err != nil {
		panic(err)
	}
	defer file.Close()

	// Create a new buffered reader.
	reader := bufio.NewReader(file)

	// Read lines from the file.
	for {
		line, err := reader.ReadString('\n')
		if err != nil {
			break
		}
		fmt.Print(line)
	}
}
```

## Buffered Writing

You can also use `bufio` to write data efficiently:

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	// Open a file for writing.
	file, err := os.Create("output.txt")
	if err != nil {
		panic(err)
	}
	defer file.Close()

	// Create a new buffered writer.
	writer := bufio.NewWriter(file)

	// Write some lines to the file.
	for i := 0; i < 10; i++ {
		fmt.Fprintf(writer, "This is line %d\n", i)
	}

	// Ensure all buffered operations are completed.
	err = writer.Flush()
	if err != nil {
		panic(err)
	}
}
```

## Buffered Scanning

The `bufio.Scanner` type provides a convenient way to read data line-by-line:

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func main() {
	// Open a file for reading.
	file, err := os.Open("data.log")
	if err != nil {
		panic(err)
	}
	defer file.Close()

	// Create a scanner.
	scanner := bufio.NewScanner(file)

	// Scan each line.
	for scanner.Scan() {
		fmt.Println(scanner.Text())
	}

	// Check for errors during scanning.
	if err := scanner.Err(); err != nil {
		fmt.Println("Error reading file:", err)
	}
}
```

## Best Practices

- Use buffered I/O for improving performance when reading from or writing to files.
- Always handle errors returned by `bufio` functions, especially during reading and writing.
- When using `bufio.Writer`, ensure to call `Flush()` to write any buffered data to the underlying writer.
- Use `bufio.Scanner` for line-by-line reading when processing logs or similar text files.

## Common Pitfalls

- Forgetting to call `Flush()` on a `bufio.Writer`, which might result in not all data being written to storage.
- Not checking for errors immediately after `Scan()` or `ReadString()` calls, which might lead to silent failures.
- Assuming the buffer size provided by default is sufficient; sometimes adjusting it can lead to better performance.

## Performance Tips

- Adjust the buffer size in `bufio.Reader` or `bufio.Writer` to best suit the size of your data to reduce overhead.
- When scanning through files, customize the split function with `bufio.Scanner.Split()` to efficiently parse different data formats.
- Avoid using `bufio.Scanner` for extremely large tokens as they may exceed the buffer capacity. Use `bufio.Reader` with custom logic in such scenarios.