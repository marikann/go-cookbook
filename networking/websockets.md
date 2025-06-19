---
title: 'Using WebSockets in Go'
description: 'Learn how to implement WebSockets in Go using the gorilla/websocket package for real-time communication.'
date: '2025-03-24'
category: 'Networking'
---

WebSockets provide a full-duplex communication channel over a single TCP connection, useful for real-time applications. In Go, the `gorilla/websocket` package is a popular choice to work with WebSockets.

## Simple WebSocket Server

The following example demonstrates how to set up a basic WebSocket server using the `gorilla/websocket` package:

```go
package main

import (
	"fmt"
	"net/http"
	"github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
	CheckOrigin: func(r *http.Request) bool {
		return true // Allow connections from any origin
	},
}

func handleConnections(w http.ResponseWriter, r *http.Request) {
	ws, err := upgrader.Upgrade(w, r, nil)
	if err != nil {
		fmt.Println(err)
		return
	}
	defer ws.Close()

	for {
		// Read message from client.
		messageType, msg, err := ws.ReadMessage()
		if err != nil {
			break
		}
		// Print received message.
		fmt.Printf("Received: %s\n", msg)

		// Write message back to client.
		err = ws.WriteMessage(messageType, msg)
		if err != nil {
			break
		}
	}
}

func main() {
	http.HandleFunc("/ws", handleConnections)
	fmt.Println("WebSocket server started on :8080")
	err := http.ListenAndServe(":8080", nil)
	if err != nil {
		fmt.Println("ListenAndServe: ", err)
	}
}
```

## Client Example

Here's how to implement a WebSocket client in Go using the same package:

```go
package main

import (
	"fmt"
	"log"
	"github.com/gorilla/websocket"
)

func main() {
	c, _, err := websocket.DefaultDialer.Dial("ws://localhost:8080/ws", nil)
	if err != nil {
		log.Fatal("dial:", err)
	}
	defer c.Close()

	// Send a message to the server.
	err = c.WriteMessage(websocket.TextMessage, []byte("Hello from client"))
	if err != nil {
		log.Println("write:", err)
		return
	}

	// Read message from the server.
	_, message, err := c.ReadMessage()
	if err != nil {
		log.Println("read:", err)
		return
	}
	fmt.Printf("Received: %s\n", message)
}
```

## Best Practices

- **Origin Checking:** Always validate the origin of requests to prevent Cross-Site WebSocket Hijacking. Customize `CheckOrigin` to allow only trusted domains.
- **Error Handling:** Implement robust error handling. Use logger instead of `fmt.Println` for better diagnostic control.
- **Concurrency:** Handle WebSocket connections concurrently to ensure responsiveness.

## Common Pitfalls

- **Unclosed Connections:** Failing to close WebSocket connections can lead to resource leakage. Utilize `defer` to ensure proper closure.
- **Origin Checks Omission:** Omitting origin checks can open up security vulnerabilities.
- **Blocking Operations:** Performing blocking operations in the read/write loop without using goroutines can cause bottlenecks, hindering real-time performance.

## Performance Tips

- **Goroutines for Heavy Tasks:** Offload heavy processing tasks to separate goroutines to keep the WebSocket pipeline non-blocked.
- **Compression:** Use WebSocket compression if supported by the client to reduce the amount of data transferred.
- **Batch Messages:** For high-frequency updates, consider batching messages together to reduce the number of writes.

By following these guidelines and leveraging the `gorilla/websocket` package, you can effectively implement WebSocket-based real-time communication in your Go applications.