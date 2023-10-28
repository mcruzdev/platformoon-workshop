# Creating a Quarkus REST API: Hands-On

In this tutorial, we will create a Quakus REST API with Java.

## Prerequisites

- [Quarkus CLI](https://pt.quarkus.io/guides/cli-tooling)

I am using the version **3.1.0.Final**.

### Creating the Quarkus application

Let's create our API with quarkus CLI:

```bash
quarkus create app com.github.platformoon:platformoon-api
```

- `com.github.platformoon` is the groupId.

- `platformoon-api` is the artifactId.


### Running our API

Go to the `platformoon-api` directory.

And execute:

```bash
quarkus dev
```

The output should looks something like this:

```bash
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
2023-10-27 21:28:54,604 INFO  [io.quarkus] (Quarkus Main Thread) platformoon-api 1.0.0-SNAPSHOT on JVM (powered by Quarkus 3.5.0) started in 1.742s. Listening on: http://localhost:8080

2023-10-27 21:28:54,608 INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
2023-10-27 21:28:54,608 INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [cdi, resteasy-reactive, smallrye-context-propagation, vertx]

```

If you access your browser at: [http://localhost:8008/hello](http://localhost:8080/hello), you will get a response from the API.

### Installing RESTEasy Reactive Jackson

Let's install Jackson to get support for JSON Serialisation:


```bash
quarkus ext add io.quarkus:quarkus-resteasy-reactive-jackson
``` 

### Creating the Resource

```java title="com.github.platformoon.presentation.resource.ApplicationResource"
package com.github.platformoon.presentation.resource;

import java.net.URI;
import java.util.UUID;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.github.platformoon.presentation.resource.request.CreateApplicationRequest;

import jakarta.ws.rs.POST;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.core.Response;

@Path("/applications")
@Consumes(MediaType.APPLICATION_JSON)
@Produces(MediaType.APPLICATION_JSON)
public class ApplicationResource {

    private static final Logger LOGGER = LoggerFactory.getLogger(ApplicationResource.class);

    @POST
    public Response createApplicationHandler(final CreateApplicationRequest request) {
        LOGGER.info("createApplicationHandler: {}", request);

        return Response
            .created(
                URI.create("/applications/" + UUID.randomUUID().toString()))
            .build();
    }
}
```

### Creating the DTO
```java title="com.github.platformoon.presentation.resource.request.CreateApplicationRequest"

package com.github.platformoon.presentation.resource.request;

public record CreateApplicationRequest(String name, String technology) {}
```

### Testing the resource

Execute the application with `quarkus dev` and make a request to the new endpoint:

```bash
curl -v --request POST \
  --url http://localhost:8080/applications \
  --header 'Content-Type: application/json' \
  --data '{
	"name": "application",
	"technology": "JAVA",
	"type": "API"
}'
```

We can observe the response header 'Location' containing the resource URI. If we check the Quarkus log, we may find something like this:

```bash
2023-10-27 22:09:29,646 INFO  [com.git.pla.pre.res.ApplicationResource] (executor-thread-1) createApplicationHandler: CreateApplicationRequest[name=application, technology=JAVA, type=API]
```

### Done

Let's return to the Platformoon CLI and implement an HTTP call to this new endpoint. Go to the [Calling REST API using GoLang tutorial](/tutorials/calling-rest-api-using-go-lang/).