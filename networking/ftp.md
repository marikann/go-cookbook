---
title: 'Working with FTP in Go'
description: 'Learn how to interact with FTP servers in Go using the goftp library.'
date: '2025-03-24'
category: 'Networking'
---

Interacting with FTP (File Transfer Protocol) servers in Go can be accomplished using libraries that simplify TCP-based file transfers. One popular library for handling FTP in Go is the `goftp` package.

## Connecting to an FTP Server

Here's a simple example of establishing a connection with an FTP server and listing files in the server's root directory using the `goftp` library:

```go
package main

import (
	"fmt"
	"log"

	"github.com/goftp/client"
)

func main() {
	config := client.Config{
		User:     "username",
		Password: "password",
	}
	
	conn, err := client.DialConfig(config, "ftp.example.com:21")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Quit()

	entries, err := conn.List("/")
	if err != nil {
		log.Fatal(err)
	}

	for _, entry := range entries {
		fmt.Println(entry.Name)
	}
}
```

## Uploading a File to an FTP Server

The following example demonstrates how to upload a file to the FTP server:

```go
package main

import (
	"log"
	"os"

	"github.com/goftp/client"
)

func main() {
	config := client.Config{
		User:     "username",
		Password: "password",
	}
	
	conn, err := client.DialConfig(config, "ftp.example.com:21")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Quit()

	file, err := os.Open("localfile.txt")
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()

	err = conn.Stor("/remote/remoteFile.txt", file)
	if err != nil {
		log.Fatal(err)
	}
}
```

## Downloading a File from an FTP Server

Here's how you can download a file from an FTP server:

```go
package main

import (
	"log"
	"os"

	"github.com/goftp/client"
)

func main() {
	config := client.Config{
		User:     "username",
		Password: "password",
	}
	
	conn, err := client.DialConfig(config, "ftp.example.com:21")
	if err != nil {
		log.Fatal(err)
	}
	defer conn.Quit()

	file, err := os.Create("downloaded.txt")
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()

	err = conn.Retr("/remote/remoteFile.txt", file)
	if err != nil {
		log.Fatal(err)
	}
}
```

## Best Practices

- Ensure that you correctly configure the FTP connection with proper credentials and host details.
- Always close connections and files using `defer` to avoid resource leaks.
- Use secure FTP (FTPS) for sensitive data transfers to enhance security.
- Always verify the success of operations like file uploads, downloads, and directory listings.

## Common Pitfalls

- Not handling connection errors or failed authentication gracefully.
- Forgetting to close opened files or connections, leading to resource leaks.
- Assuming directory paths and file names exist without validating them on the server.
- Not splitting large files into smaller parts, which can lead to timeouts or failures during transfers.

## Performance Tips

- Use persistent FTP connections for multiple operations to avoid the overhead of reconnecting.
- Consider the use of parallel file transfers for improved throughput, keeping in mind potential connection limits.
- Monitor and log performance metrics to identify bottlenecks or errors in the transfer process.