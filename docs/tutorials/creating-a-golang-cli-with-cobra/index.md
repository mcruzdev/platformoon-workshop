# Creating a GoLang with Cobra: Hands-On

In this tutorial, we will create a command-line application in GoLang.

## Prerequisites

- [GoLang version 1.20 or higher](https://go.dev/doc/install)

### Creating the GoLang application

Starting a GoLang application is quite simple; you just need to create a folder and run the command `go mod init [module-path]`.

Let's create:

```bash
mkdir platformoon-cli && cd platformoon-cli
go mod init github.com/mcruzdev/platformoon-cli
```

If you type `tree .` in terminal, the output should looks like:

```bash
.
└── go.mod
```

Let's see the `go.mod` content:

```mod
module github.com/mcruzdev/platformoon-cli

go 1.20
```

### Installing [Cobra dependency](https://github.com/spf13/cobra)

Cobra overview:

> Cobra is a library providing a simple interface to create powerful modern CLI interfaces similar to git & go tools.

Getting the dependency with `go`:

```bash
go get -u github.com/spf13/cobra@latest
```

### Creating the root command (platformoon)

Let's see the initial code:

``` go
package main

import (
	"fmt"
	"log"
	"os"

	"github.com/spf13/cobra"
)

func main() { // (1)
	rootCmd := NewPlatformoonRoot()
	if err := rootCmd.Execute(); err != nil {
		fmt.Fprintln(os.Stderr, err)
		os.Exit(1)
	}
}

func NewPlatformoonRoot() *cobra.Command { // (2)
	var root = &cobra.Command{
		Use:   "platformoon",
		Short: "Platformoon CLI facilitates programmers lifes",
		Run: func(cmd *cobra.Command, args []string) {
			log.Println("platformoon works")
		},
	}

	return root
}
```

1.  main GoLang function
2.  Factory function to create the root `*cobra.Command`

### Testing the CLI

Run the application:

```bash
go run cmd/platformoon/platformoon.go
```

The output should looks something like this:

```bash
2023/10/27 20:47:56 platformoon works
```

Great, let's create the complete command `platformoon application create --name [application-name]`.

### Adding subcommands to the platformoon command

```go
func NewPlatformoonRoot() *cobra.Command {
	var root = &cobra.Command{
		Use:   "platformoon",
		Short: "Platformoon CLI facilitates programmers lifes",
		Run: func(cmd *cobra.Command, args []string) {
			log.Println("platformoon works")
		},
	}

	root.AddCommand(NewApplicationCmd()) // (1)

	return root
}

func NewApplicationCmd() *cobra.Command { // (2)
	var cmd = &cobra.Command{
		Use:   "application",
		Short: "This command consists of multiple subcommands to interact with applications",
		Run: func(cmd *cobra.Command, args []string) {
			log.Println("platformoon application works")
		},
	}

	cmd.AddCommand(NewApplicationCreateCmd()) // (3)
	return cmd
}

func NewApplicationCreateCmd() *cobra.Command { // (4)
	var cmd = &cobra.Command{
		Use:   "create",
		Short: "This command is used to create a new application",
		Run: func(cmd *cobra.Command, args []string) {
			log.Println("platformoon application create works")
		},
	}

	return cmd
}

```

1.	Add `[application]` command as subcommand
2.	Create `application` command
3.	Add `create` command as `application` subcommand
4.	Create `create` command 

### Building the application

To test how the end user experience will be, let's build our CLI.

```bash
go build -o platformoon cmd/platformoon/platformoon.go 
```

### Testing the application

Add permission to execute:

```bash
chmod +x platformoon
```

Execute the following commands and see the result:

```bash
./platformoon application
```

```bash
./platformoon application create
```

### Adding the logic to send HTTP request to REST API

So far, the API doesn't exist yet, so let's create it now.[ Access this tutorial to create a Quarkus application](/tutorials/creating-a-quarkus-rest-api) that will serve as our REST API.

!!! NOTE

	Don't worry, we'll return to working with our CLI as soon as we create our REST API.

	Commit and push your changes to access later.