---
title: 'Interfaces'
description: 'Learn about interfaces in Go, how to use them effectively, and some common use cases'
date: '2025-03-24'
category: 'Basics'
---

Interfaces in Go provide a powerful way to implement polymorphism and define behaviors. They are essentially a contract that a type must satisfy. This guide will cover the basics of using interfaces in Go.

## Defining and Using Interfaces

Here's a simple example to demonstrate how interfaces are defined and used:

```go
package main

import (
	"fmt"
)

// Define an interface.
type Greeter interface {
	Greet() string
}

// Define a type that satisfies our interface.
type Human struct {
	Name string
}

func (h Human) Greet() string {
	return "Hello!"
}

// Another type that satisfies the interface.
type Cow struct {
	Name string
}

func (c Cow) Greet() string {
	return "Moo?"
}

func main() {
	var g Greeter
	human := Human{Name: "Alex"}
	cow := Cow{Name: "Milka"}

	g = human
	fmt.Println(g.Greet()) // Output: Hello!

	g = cow
	fmt.Println(g.Greet()) // Output: Moo!
}
```

## Type Assertion

Using type assertions, you can retrieve the concrete type from an interface variable:

```go
package main

import (
	"fmt"
)

// An interface and a type.
type Vehicle interface {
	Drive() string
}

type Car struct {
	Model string
}

func (c Car) Drive() string {
	return "All in!"
}

func checkVehicle(v Vehicle) {
	if car, ok := v.(Car); ok {
		fmt.Printf("Car model: %s\n", car.Model)
	} else {
		fmt.Println("Unknown type")
	}
}

func main() {
	var v Vehicle
	v = Car{Model: "Renault"}
	checkVehicle(v)  // Output: Car model: Renault
}
```

## Best Practices

- Design interfaces to define high-level behaviors, keeping them small and focused.
- Use interface names that reflect what they do, often by using verbs like `Reader`, `Writer`, or `Formatter`.
- Utilize type assertion only when necessary, and handle cases where it might fail gracefully.

## Common Pitfalls

- Avoid large interfaces; they make code less flexible and harder to test.
- Don't mix interface and concrete type methods within the same implementation scope.
- For beginners, understanding error messages related to interface satisfaction can be non-intuitive. Carefully match method signatures.

## Performance Tips

- Use interfaces to decouple abstractions, but be cautious with high-frequency interface invocations which might introduce indirection costs.
- Interfaces have a pointer and a type, which requires access to memory. Consider the data flow and bottlenecks when designing systems.
- Profile your applications to see if interface indirection is a bottleneck.
- Consider avoiding interface usage in performance-critical paths unless polymorphism is required and brings significant design benefits.