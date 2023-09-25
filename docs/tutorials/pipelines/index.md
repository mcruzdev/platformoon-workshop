# Getting Started

To get started with pipelines and Tekton you need a Kubernetes cluster.

We are use KinD installed on this tutorial [Installing KinD](../../tutorials/index.md)

## Installing Tekton

### Why Tekton?

I wanted to learn more about Tekton and I decided to use it on this project. Tekton is a powerful and flexible open-source framework for creating CI/CD systems, allowing developers to build, test, and deploy across cloud providers and on-premise systems. It is built on Kubernetes, providing the benefits of an open-source, cloud-native experience from pipeline creation to execution.


For more details see the official [documentation](https://tekton.dev/docs/concepts/overview/).

## Intalling Tekton Pipelines

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

## Installing Tekton Trigger

```bash
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
kubectl apply --filename \
https://storage.googleapis.com/tekton-releases/triggers/latest/interceptors.yaml
```

## Installing Tekton Dashboard

```bash
kubectl apply -f https://github.com/tektoncd/dashboard/releases/download/v0.33.0/release.yaml
```

You can access the dashboard using the following command:

```bash
kubectl port-forward svc/tekton-dashboard  -n tekton-pipelines 9097:9097
```

## Installing git-clone and buildpacks tasks from Tekton Hub

In this step we will install two tekton tasks from Tekton Hub, the git-clone and the buildpacks.

`git-clone` is a task that clones a git repository into a `Workspace` volume.

`buildpacks` is a task that builds an application using Cloud Native Buildpacks.


```bash
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/master/task/buildpacks/0.6/buildpacks.yaml
```

## Installing Tekton Objects

```bash
kubectl apply -f tekton/ci
```

## Installing Ingress Controller for Tekton

```bash
kubectl apply -f tekton/ingress.yaml
```

Now we have a webhook configured and we can run the pipeline using the following command:

```bash
curl --request POST \
  --url http://localhost/tekton \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/2023.5.8' \
  --data '{
	"url": "https://github.com/platformoon/destroyer-server.git",
	"revision": "main",
	"action": "build"
}'
```