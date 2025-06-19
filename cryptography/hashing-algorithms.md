---
title: 'Hashing Algorithms in Go'
description: 'Learn how to use hashing algorithms in Go using the built-in crypto package'
date: '2025-03-24'
category: 'Cryptography'
---

Hashing algorithms are widely used in cryptography to ensure data integrity and to securely store passwords. Go offers support for various hashing algorithms through its `crypto` package.

## Basic Hashing with SHA-256

Here's a straightforward example of how to create a hash of a string using SHA-256:

```go
package main

import (
	"crypto/sha256"
	"fmt"
)

func main() {
	input := "golang crypto example"
	hash := sha256.New()
	hash.Write([]byte(input))
	hashed := hash.Sum(nil)
	fmt.Printf("SHA-256 hash: %x\n", hashed)
}
```

## Using SHA-512

If a stronger hash is required, you can use SHA-512 like this:

```go
package main

import (
	"crypto/sha512"
	"fmt"
)

func main() {
	input := "golang crypto example"
	hash := sha512.New()
	hash.Write([]byte(input))
	hashed := hash.Sum(nil)
	fmt.Printf("SHA-512 hash: %x\n", hashed)
}
```

## MD5 Hashing

Though not recommended for secure hashing due to vulnerabilities, MD5 can still be used for checksum purposes:

```go
package main

import (
	"crypto/md5"
	"fmt"
)

func main() {
	input := "golang crypto example"
	hash := md5.New()
	hash.Write([]byte(input))
	hashed := hash.Sum(nil)
	fmt.Printf("MD5 hash: %x\n", hashed)
}
```

## Best Practices

- Prefer SHA-256 or SHA-512 over MD5 for security-critical applications.
- Always use a cryptographic salt in combination with hashing for storing passwords.
- Use `crypto/hmac` for creating hash-based message authentication codes to ensure data integrity.

## Common Pitfalls

- Using MD5 for secure hashing purposes due to its vulnerability to collision attacks.
- Not handling potential errors when writing to the hash function or reading from input.
- Forgetting to initialize a new hash object for every new hashing operation results in incorrect hash values.

## Performance Tips

- For large data sets, consume data in chunks when hashing to reduce memory consumption.
- Consider using Go's buffered I/O constructs if you are hashing files or streams.
- Use specific protocol optimizations (like `SHA-224` or `SHA-384`) if applicable to balance between performance and security.