# Argo proof of concept

## Prerequisites

You must install and configure the following tools before moving forward

* minikube 
* kubectl
* kubernetes must be running

## Install Argo Workflows

To get started quickly, you can use the quick start manifest which will install Argo Workflow as well as some commonly used components:

```bash
kubectl create ns argo
kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo/stable/manifests/quick-start-postgres.yaml
```

If you are running Argo Workflows locally (e.g. using Minikube), open a port-forward so you can access the namespace:

```bash
kubectl -n argo port-forward deployment/argo-server 2746:2746
```

This will serve the user interface on http://localhost:2746

Next, Download the latest Argo CLI from releases page and test installation

```bash
argo version
```
## Submit Argo Workflows

Finally, submit an example workflow:

```bash
argo submit -n argo --watch hello-world.yaml
```

