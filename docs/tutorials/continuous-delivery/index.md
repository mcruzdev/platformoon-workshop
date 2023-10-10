# Configuring Github Webhooks

In this tutorial we will configure a Github Webhook and listen all events from Github using Quarkus and `ngrok` toolings.

## Installing and configuring ngrok

If you are not familiar with `ngrok`, see more details [here](https://ngrok.com/).

### Configuring ngrok

Before continuing, you need to install `ngrok`, to see how to do it, follow this [Getting Started tutorial](https://dashboard.ngrok.com/get-started/setup).

## Creating a REST resource

Let's go to the our application. I created a REST resource just to log the request body from Github event.

```java
package com.github.platformoon.presentation.resources;

import jakarta.ws.rs.Consumes;
import jakarta.ws.rs.Path;
import jakarta.ws.rs.Produces;
import jakarta.ws.rs.core.MediaType;
import jakarta.ws.rs.core.Response;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Path("/gh/webhooks")
@Consumes(value = MediaType.APPLICATION_JSON)
@Produces(value = MediaType.APPLICATION_JSON)
public class GithubWebhookResource {

    private static final Logger LOGGER = LoggerFactory.getLogger(GithubWebhookResource.class);

    public Response listen(String command) {
        LOGGER.info(command);
        return Response.ok().build();
    }
}
```

### Running Quarkus application

Is very simple to run a Quarkus application, I am using `quarkus` CLI to do it.

Execute the following command at the application root directory:

```bash
quarkus dev
```

The output should looks like it:

```bash
2023-10-10 09:28:37,871 INFO  [io.qua.kub.cli.dep.DevServicesKubernetesProcessor] (build-39) Dev Services for Kubernetes started. Other Quarkus applications in dev mode will find the cluster automatically.
__  ____  __  _____   ___  __ ____  ______ 
 --/ __ \/ / / / _ | / _ \/ //_/ / / / __/ 
 -/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \   
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/   
2023-10-10 09:28:39,746 INFO  [io.quarkus] (Quarkus Main Thread) apps 1.0-SNAPSH2023-10-10 09:28:39,749 INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
2023-10-10 09:28:39,750 INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [agroal, cdi, hibernate-orm, jdbc-mysql, kubernetes-client, narayana-jta, rest-client, rest-client-jackson, resteasy, smallrye-context-propagation, vertx]
```

### Running ngrok

After running the Quarkus application, will be able to execute the `ngrok`, to do it execute:

```bash
ngrok http 8080
```

The `http` say to the `ngrok` to use HTTP protocol and the 8080 indicates to proxy the request to the port 8080 (the default port used by Quarkus). The output should looks like it:

```bash 
Introducing Always-On Global Server Load Balancer: https://ngrok.com/r/gslb                                                                           
                                                                                                                                                      
Session Status                online                                                                                                                  
Account                       matheuscruz.dev@gmail.com (Plan: Free)                                                                                  
Version                       3.3.5                                                                                                                   
Region                        South America (sa)                                                                                                      
Latency                       13ms                                                                                                                    
Web Interface                 http://127.0.0.1:4040                                                                                                   
Forwarding                    https://1234-1234-7f0-90c0-6628-abcd-65c-1234-5fc6.ngrok-free.app -> http://localhost:8080                              
                                                                                                                                                      
Connections                   ttl     opn     rt1     rt5     p50     p90                                                                             
                              0       0       0.00    0.00    0.00    0.00       
```

## Configuring the org Webhook

Now, that we have the URL necessary to send the Webhook to our platform, let's configure it on Github.

1. Go to Github > Settings > Organizations.

2. Select the Organization that you want, and click in Settings.

3. Go to Webhooks.

4. Click on `Add webhook` button.

5. Add the Payload URL with the `ngrok` URL (in my case was necessary to add the path `/gh/webhooks` for listen it).

6. Click on `Add webhook` button to save.

Done, now we can listen for all Github events and manage it.