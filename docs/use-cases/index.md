# Introduction

To begin, I'd like to outline a list of use cases for which I would like the platform to cater to my needs.

## US-001

As a developer, I want to be able to create my application, download it, and then immediately start developing the business rules without having to worry about environment configurations, so that I can focus on the business itself and not on technical details and code boilerplate.

### How to address this use case?

!!! Note

    I'm not going to dwell on the various ways to solve this use case here; I'll go for the simplest one to implement, and of course, it should involve the technologies I want to learn.

- **User interface**: Create a GoLang CLI with Cobra.
- **API**: Create a Quarkus application to serve as a REST API to communicate with Github for creating a new repository.

### How will the user use the CLI?


To create an application, the user will type:

```bash
platformoon application create --name book-api
```

Let's address the use case **US001** here.