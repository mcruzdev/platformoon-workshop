# Calling REST API Using GoLang: Hands-On

### Creating a HTTP Client to perform requests

Let's create a simple file that will be used to make HTTP request:

```go title="internal/httpclient/httpclient.go"
package httpclient

import (
	"bytes"
	"encoding/json"
	"net/http"
)

func Post(url string, body interface{}) (*http.Response, error) {

    marshal, err := json.Marshal(body)
	if err != nil {
		panic(any(err))
	}

	byteArr := []byte(marshal)

	return http.Post(url, "application/json", bytes.NewBuffer(byteArr))

}

```

### Creating the application responsible for Application

This module contain all things about application.

Let's get the implementation:

```go title="internal/application/application.go"
package application

import (
	"log"
	"net/http"

	"github.com/mcruzdev/platformoon-cli/internal/httpclient"
)

type ApplicationRequest struct {
	Name       string `json:"name"`
	Type       string `json:"type"`
	Technology string `json:"technology"`
}

func NewApplicationRequest(name, appType, technology string) *ApplicationRequest {
	return &ApplicationRequest{
		Name:       name,
		Technology: technology,
		Type:       appType,
	}
}

func CreateNewApplication(name, appType, technology string) {
	ar := NewApplicationRequest(name, appType, technology)
	r, err := httpclient.Post("http://localhost:8080/applications", ar)

	if err != nil {
		panic(any(err))
	}

	if r.StatusCode == http.StatusCreated {
		log.Printf("Application created: %s", r.Header["Location"])
	}
}

```

### Change application create command

Finally, let's change the handle for `platformoon application create` command:

```go title="cmd/platformoon/platformoon.go"
func NewApplicationCreateCmd() *cobra.Command {
	var cmd = &cobra.Command{
		Use:   "create",
		Short: "This command is used to create a new application",
		Run: func(cmd *cobra.Command, args []string) {
			log.Println("platformoon application create works")
			application.CreateNewApplication("application", "API", "JAVA") // (1)
		},
	}

	return cmd
}
```

1.  This values are hard-coded, we will change it later.

### Calling the REST API

Now, that all things are okay, let's execute our CLI:

!!! NOTE

    Check if the REST API is running.

```bash
go run cmd/platformoon/platformoon.go application create
```

The output should looks something like it:

```bash
2023/10/27 22:44:54 platformoon application create works
2023/10/27 22:44:54 Application created: [http://localhost:8080/applications/06ce9936-4a74-4328-b2f7-f4e40e310706]
```

### Thank you!

Done! The next step is to add parameters to the CLI, [follow this tutorial](/tutorials/using-parameters-on-cobra-cli/) to do it.