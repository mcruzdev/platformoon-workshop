# Features

## Allow the user to create an application through the CLI

The command should be something like this:

```bash
    platformoon create application --name=app --kind=backend
```

The result of this operation should be:

- The repository created on Github
- The application stored to be retrieved later

### What is necessary to do it?

- Create an organization on Github
- Get the token from Github
- Create a repository on Github for the API
- Create a repository for the CLI

!!! abstract "Note"
    For now, I will not install Keycloak and the communication between the CLI and the API will be unsafe.