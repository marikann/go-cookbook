---
title: 'Debugging with Delve'
description: 'Learn how to effectively debug Go applications using the Delve debugger with practical examples.'
date: '2025-03-24'
category: 'Debugging'
---

Delve is a powerful debugger for the Go programming language, widely regarded as the standard for debugging Go applications. It helps developers set breakpoints, examine goroutines, inspect variables, and step through the code to diagnose issues.

## Basic Debugging with Delve

Here's a simple example to showcase how to start debugging a Go application using Delve:

1. First, install Delve:

   ```sh
   go install github.com/go-delve/delve/cmd/dlv@latest
   ```

2. Start a Delve session on a Go executable:

   ```sh
   dlv debug main.go
   ```

   This will compile and launch the application within a debugging session.

3. Set a breakpoint at a specific function:

   ```sh
   (dlv) break main.main
   ```

4. Run the application:

   ```sh
   (dlv) continue
   ```

5. When the breakpoint is hit, inspect variables:

   ```sh
   (dlv) print myVariable
   ```

6. Step through the code:

   ```sh
   (dlv) next
   (dlv) step
   ```

## Debugging a Running Process

Delve can also attach to a running process if you have its PID:

1. Start the application normally.

2. Find the process ID (PID):

   ```sh
   ps aux | grep myAppName
   ```

3. Attach Delve to the process:

   ```sh
   dlv attach <PID>
   ```

4. Set breakpoints, inspect variables, and use other commands as needed.

## Advanced Features

Delve offers more advanced capabilities, such as viewing goroutine information and dumping stack traces:

- **List Goroutines:**

  ```sh
  (dlv) goroutines
  ```

- **Stack Trace of a Goroutine:**

  ```sh
  (dlv) goroutine <ID> stack
  ```

- **View Source Code:**

  ```sh
  (dlv) list
  ```

- **Evaluate Expressions:**

  ```sh
  (dlv) eval someFunction(param)
  ```

## Best Practices

- Use `next` to move to the next line and `step` to dive into functions when necessary.
- Always try to filter logs to reduce noise in output when debugging complex applications.

## Common Pitfalls

- Forgetting to build with optimizations disabled (`-gcflags="all=-N -l"`) can make debugging difficult due to compiler optimizations.
- Not checking if Delve has the necessary permissions to attach to processes; ensure proper permissions (e.g., run with `sudo` if needed or adjust security settings).
- Misaligned expectations of variable values due to unexpected program state or optimization.

## Performance Tips

- Don’t overuse breakpoints, especially in tight loops, as it can drastically slow down your application under debug.
- Limit logging in debug sessions to focus on relevant output and improve clarity and performance.
- Use conditional breakpoints to pause execution only when specific conditions are met, minimizing unnecessary disruptions.

Delve is an essential tool in a Go developer's toolkit, offering robust debugging capabilities to efficiently diagnose and solve problems in Go applications. By mastering its features, you can significantly improve your debugging workflow.