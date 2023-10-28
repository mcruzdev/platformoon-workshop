# Using Parameters on Cobra CLI: Hands-On

### Adding parameters

The Cobra documentation says that:

> There are two different approaches to assign a flag.
> **Persistent** and **Local Flags**.

**Persistent**

> A flag can be 'persistent', meaning that this flag will be available to the command it's assigned to as well as every command under that command. For global flags, assign a flag as a persistent flag on the root.

**Local Flags**

> A flag can also be assigned locally, which will only apply to that specific command.

### Required flags

We will use local flags, and we need to ensure that all flags are provided.

> Flags are optional by default. If instead you wish your command to report an error when a flag has not been set, mark it as required.

In our case, we need to be sure that `--name`, `--type` and `--technology` are provided together, then it makes sense for us.

### Changing the code

Let's change the `NewApplicationCreateCmd()` function.

```go title="cmd/platformoon/platformoon.go"
func NewApplicationCreateCmd() *cobra.Command {
	var cmd = &cobra.Command{
		Use:   "create [OPTIONS]",
		Short: "This command is used to create a new application",
		Run: func(cmd *cobra.Command, args []string) {
			log.Println("platformoon application create works")
			application.CreateNewApplication("application", "API", "JAVA")
		},
	}

	cmd.Flags().StringVarP(&applicationName, "name", "n", "", "Application name")
	cmd.Flags().StringVarP(&technology, "technology", "t", "", "Technology of the application, e.g., (java, go)")
	cmd.Flags().StringVarP(&appType, "kind", "k", "", "Kind of the application, e.g., web, library")

	return cmd
}

```

`StringVarP` method:

1. "name" means the flag name
2. "n" means the short name
3. "" means default value is empty
4. "Application name" means the help message for this flag 

We can see that we are using three variables: `applicationName`, `technology` and `appType`. Let's declare at the top of the file.

```go title="cmd/platformoon/platformoon.go"
var (
	applicationName string
	technology      string
	appType         string
)
```

Now, all options are required, let's use them!

```go title="cmd/platformoon/platformoon.go"
Run: func(cmd *cobra.Command, args []string) {
    log.Println("platformoon application create works")
    application.CreateNewApplication(applicationName, appType, technology) // (1)
},
```

1.  Replace hard-coded values with the variables

### Validation

In the real world, when a flag is not valid, we don't even make the REST API call, needlessly expending time and computational resources. It would be much better to validate on the client side first, as it's a straightforward task like data validation.

Let's add validation to the method that creates the struct `ApplicationRequest` for the request.

```go title="internal/application/application.go"

func NewApplicationRequest(name, appType, technology string) *ApplicationRequest {

	technologies := []string{"JAVA", "GO"} // (1)
	appTypes := []string{"API", "LIBRARY"} // (2)

	if len(name) < 4 {
		fmt.Println("ERROR: The application name must be at least 4 characters long.")
		os.Exit(1)
	}

	if !containsIgnoringCase(appType, appTypes) {
		fmt.Println(fmt.Sprintf("ERROR: The application type must be either: %s.", strings.Join(appTypes, ", ")))
		os.Exit(1)
	}

	if !containsIgnoringCase(technology, technologies) {
		fmt.Println(fmt.Sprintf("ERROR: The application type must be either: %s.", strings.Join(appTypes, ", ")))
		os.Exit(1)
	}

	return &ApplicationRequest{
		Name:       name,
		Technology: technology,
		Type:       appType,
	}
}
```

1.	Supported technologies
2.	Supported application types

Done! now we have all things complete, feel free to make refactor and more validation or changes!

!!! NOTE

	It's much easier to have validation only on the backend, but when developing the critical part, if the backend allows a new type of technology, the CLI needs to adapt as well. For our platform, this isn't a significant issue, but it's important to acknowledge that there are trade-offs.

### Calling the REST API

Be sure that the REST API is running and execute the following command:

```bash
go run cmd/platformoon/platformoon.go application create -n java -k api -t java
```

The output should look something like this:

```
2023/10/28 06:59:27 platformoon application create works
2023/10/28 06:59:27 Application created: [http://localhost:8080/applications/8c2f2909-70e5-4596-8933-665a1e23b10e]
```