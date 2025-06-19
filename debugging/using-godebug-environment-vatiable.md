---
title: 'Using GODEBUG Environment Variable'
description: 'Understand and utilize the GODEBUG environment variable in Go for debugging and performance tuning'
date: '2025-03-24'
category: 'Tools'
---

The `GODEBUG` environment variable is an essential tool for developers looking to debug and tune Go programs. It allows for the configuration of various runtime debugging facilities. This snippet provides an overview and usage examples for the `GODEBUG` variable.

## Basic Use of GODEBUG

To utilize the `GODEBUG` environment variable, set it before running your Go application. Here is a basic usage example:

```bash
GODEBUG=gctrace=1 ./myapp
```

This command will run `myapp` with garbage collection traces enabled, printing GC stats to the standard error.

## Common GODEBUG Settings

Here are some commonly used settings for the `GODEBUG` environment variable:

- `gctrace=1`: Enables garbage collector traces.
  
- `schedtrace=1000`: Prints scheduler traces every 1000ms.
  
- `cgocheck=2`: Enables strict checking for Go and C memory allocations (improves debugging CGo).
  
- `memprofilerate=1`: Enables memory profiling.

## Detailed Examples

### Enabling Garbage Collection Trace

This example shows how to enable garbage collection trace to monitor GC events:

```bash
export GODEBUG=gctrace=1
go run main.go
```

In this setup, every time the garbage collector is triggered, information such as elapsed time, heap size, and the number of goroutines is printed to standard error.

### Scheduler Tracing

To observe the operation of the Go scheduler, use the following configuration:

```bash
export GODEBUG=schedtrace=1000
./myapp
```

Every second (`1000` ms), this will output stats about the scheduler's state and the number of goroutines.

### CGo Pointer Check

To strictly check pointer safety when using CGo, use:

```bash
export GODEBUG=cgocheck=2
go run cgo_example.go
```

This strict mode helps in identifying unsafe pointer conversions that could cause bugs in the program.

## Best Practices

- **Start with Minimal Debug Logging**: Begin debugging with minimal logging to avoid information overload. Enable more detailed options as needed.
- **Combine Settings**: Use multiple settings to provide a comprehensive overview of application performance. For instance, combine `gctrace` and `schedtrace` to analyze memory and scheduling behavior together.
- **Develop with Testing**: Use `GODEBUG` settings during testing rather than production, as they may impact performance.

## Common Pitfalls

- **Neglecting Performance Impact**: Some `GODEBUG` settings can significantly impact performance. Ensure they're disabled in production environments.
- **Overlooking Details**: The information printed can sometimes be overwhelming. Focus on the specifics relevant to your debugging needs.

## Performance Tips

- **Use the Appropriate Rate**: Adjust the rate values in settings like `schedtrace` to balance between verbosity and information freshness.  
- **Automate Log Parsing**: Use tools or scripts to parse and analyze `GODEBUG` logs to efficiently pinpoint performance bottlenecks.

Using `GODEBUG` effectively can provide profound insights into program execution, aiding in the diagnosis and rectification of runtime and performance issues.