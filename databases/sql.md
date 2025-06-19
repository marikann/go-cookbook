---
title: 'SQL and Go'
description: 'Learn how to use SQL databases in Go using the database/sql package'
date: '2025-03-24'
category: 'Databases'
---

Go provides robust support for interfacing with SQL databases through the `database/sql` package. This guide provides examples and best practices for using SQL databases in Go efficiently.

## Basic SQL Database Connection

Here's a simple example that demonstrates how to connect to an SQL database:

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
	dsn := "username:password@tcp(localhost:3306)/dbname"
	db, err := sql.Open("mysql", dsn)
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	err = db.Ping()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Successfully connected to the database")
}
```

## Executing Queries

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/lib/pq"
)

func main() {
	connStr := "user=username dbname=dbname sslmode=disable"
	db, err := sql.Open("postgres", connStr)
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	rows, err := db.Query("SELECT id, name FROM users")
	if err != nil {
		log.Fatal(err)
	}
	defer rows.Close()

	for rows.Next() {
		var id int
		var name string
		if err := rows.Scan(&id, &name); err != nil {
			log.Fatal(err)
		}
		fmt.Printf("ID: %d, Name: %s\n", id, name)
	}

	if err := rows.Err(); err != nil {
		log.Fatal(err)
	}
}
```

## Prepared Statements

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
	dsn := "username:password@tcp(localhost:3306)/dbname"
	db, err := sql.Open("mysql", dsn)
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	stmt, err := db.Prepare("INSERT INTO users(name, age) VALUES(?, ?)")
	if err != nil {
		log.Fatal(err)
	}
	defer stmt.Close()

	res, err := stmt.Exec("Alice", 30)
	if err != nil {
		log.Fatal(err)
	}

	lastInsertId, err := res.LastInsertId()
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Inserted row with ID: %d\n", lastInsertId)
}
```

## Transactions

```go
package main

import (
	"database/sql"
	"fmt"
	"log"

	_ "github.com/lib/pq"
)

func main() {
	connStr := "user=username dbname=dbname sslmode=disable"
	db, err := sql.Open("postgres", connStr)
	if err != nil {
		log.Fatal(err)
	}
	defer db.Close()

	tx, err := db.Begin()
	if err != nil {
		log.Fatal(err)
	}

	res, err := tx.Exec("UPDATE accounts SET balance = balance - $1 WHERE id = $2", 100, 1)
	if err != nil {
		tx.Rollback()
		log.Fatal(err)
	}

	affected, err := res.RowsAffected()
	if err != nil || affected != 1 {
		tx.Rollback()
		log.Fatal("Could not update account balance")
	}

	err = tx.Commit()
	if err != nil {
		log.Fatal(err)
	}

	fmt.Println("Transaction successfully committed")
}
```

## Best Practices

- Always check for errors immediately after executing SQL commands.
- Use `defer` to close database connections (`db.Close()`) and rows (`rows.Close()`).
- Use prepared statements (`db.Prepare()`) to prevent SQL injection attacks.
- Utilize transactions for operations that require atomicity.
- Monitor database connection pool size and adjust as necessary for varying load conditions.

## Common Pitfalls

- Neglecting to handle errors, leading to unhandled exceptions.
- Forgetting to close database connections or rows, resulting in resource leaks.
- Hard coding SQL queries without parameter substitution, which can lead to SQL injection vulnerabilities.
- Not handling `rows.Err()` after iterating through query results.
- Overlooking connection pool management can lead to exhausted connections under heavy load.

## Performance Tips

- Optimize the use of prepared statements to reduce parsing and execution planning overhead.
- Utilize indexing in the database to speed up query execution, but beware of too many indexes affecting write performance.
- Use batch processing to minimize the number of database writes for bulk data ingestion tasks.
- Pool connections to avoid the overhead of establishing new connections.
- Profile queries to identify and optimize slow pieces, ensuring efficient use of database resources.