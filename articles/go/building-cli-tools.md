# Building CLI Tools in Go

Go is a strong fit for command-line tools because it produces fast, portable binaries and has a capable standard library.

## Start with a small command boundary

Keep argument parsing and process orchestration near `main`, while placing the actual work in testable functions:

```go
package main

import (
	"fmt"
	"os"
)

func run(args []string) error {
	if len(args) == 0 {
		return fmt.Errorf("usage: tool <name>")
	}

	fmt.Println("hello", args[0])
	return nil
}

func main() {
	if err := run(os.Args[1:]); err != nil {
		fmt.Fprintln(os.Stderr, err)
		os.Exit(1)
	}
}
```

## Design guidelines

- Write useful errors to standard error and return non-zero exit codes on failure.
- Support `-h` and document examples in the command help output.
- Accept input from files, standard input, or arguments where that improves composability.
- Keep output stable when users may pipe it into another command.
- Add table-driven tests for parsing, validation, and error cases.

Build and test the tool with:

```bash
go test ./...
go build -o bin/tool ./cmd/tool
```

As the tool grows, split subcommands into packages and keep shared configuration, logging, and filesystem behavior behind small interfaces.
