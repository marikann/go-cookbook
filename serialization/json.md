---
title: 'Working with JSON in Go'
description: 'Learn how to serialize and deserialize JSON in Go using the encoding/json package'
date: '2025-03-24'
category: 'Serialization'
---

JSON (JavaScript Object Notation) is a popular data interchange format due to its simplicity and readability. Go provides built-in support for encoding and decoding JSON using the `encoding/json` package. This guide provides examples for working with JSON in Go, suitable for both beginners and experienced engineers.

## Basic JSON Serialization

Here's a simple example that demonstrates how to encode a Go struct to a JSON string:

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name  string `json:"name"`
	Email string `json:"email"`
	Age   int    `json:"age"`
}

func main() {
	u := User{Name: "Alice", Email: "alice@example.com", Age: 30}

	// Serialize struct to JSON.
	userJSON, err := json.Marshal(u)
	if err != nil {
		panic(err)
	}
	fmt.Println(string(userJSON))
}
```

## JSON Deserialization

Deserializing JSON into a struct is straightforward using the `Unmarshal` function:

```go
package main

import (
	"encoding/json"
	"fmt"
)

type User struct {
	Name  string `json:"name"`
	Email string `json:"email"`
	Age   int    `json:"age"`
}

func main() {
	data := `{"name": "Bob", "email": "bob@example.com", "age": 25}`

	var u User
	// Deserialize JSON string to struct.
	err := json.Unmarshal([]byte(data), &u)
	if err != nil {
		panic(err)
	}
	fmt.Printf("Name: %s, Email: %s, Age: %d\n", u.Name, u.Email, u.Age)
}
```

## Handling Dynamic JSON

If the structure of JSON is not known at compile time, a `map` or `interface{}` can be used:

```go
package main

import (
	"encoding/json"
	"fmt"
)

func main() {
	data := `{"name": "Charlie", "email": "charlie@example.com", "age": 29, "extra": "value"}`

	var result map[string]interface{}
	err := json.Unmarshal([]byte(data), &result)
	if err != nil {
		panic(err)
	}

	for key, value := range result {
		fmt.Printf("%s: %v\n", key, value)
	}
}
```

## Best Practices

- Use `omitempty` in JSON tags to omit zero-value fields from output whenever necessary.
- Consider using custom JSON marshalling/unmarshalling if custom logic is required.

## Common Pitfalls

- Ignoring error returns from `Marshal` and `Unmarshal`.
- Using incorrect struct tags or forgetting to include them.
- Forgetting to use pointer receivers for `Unmarshal` to modify the original struct.
- Not accounting for different JSON field name case sensitivity.

## Performance Tips

- Avoid using `interface{}` when possible as it incurs type assertion overhead.
- Pre-allocate slices when the size is known, especially for large JSON arrays.
- For performance-critical applications, minimize the use of reflection by ensuring structs match JSON schema closely.
- Use streaming JSON decoding with `json.Decoder` for large JSON data to reduce memory overhead.
- Profile and optimize bottlenecks using Go's profiling tools, especially when dealing with high volumes of JSON data.