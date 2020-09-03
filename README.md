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
kubectl apply -n argo -f quick-start-postgres.yml
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
## Submit Argo Workflows - Hello World

Finally, submit an example workflow:

```bash
argo submit -n argo --watch hello-world.yaml
```

## Verify Argo Workflows - Hello World

Argo adds a new kind of Kubernetes spec called a Workflow. The above spec contains a single template called whalesay which runs the docker/whalesay container and invokes cowsay "hello world". The whalesay template is the entrypoint for the spec. The entrypoint specifies the initial template that should be invoked when the workflow spec is executed by Kubernetes. 

```bash
argo logs -n argo @latest
  _____________ 
 < hello world >
  ------------- 
     \
      \
       \     
                     ##        .            
               ## ## ##       ==            
            ## ## ## ##      ===            
        /""""""""""""""""___/ ===        
   ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~   
        \______ o          __/            
         \    \        __/             
           \____\______/  
```

## Submit Argo Workflows - DAG

As an alternative to specifying sequences of steps, you can define the workflow as a directed-acyclic graph (DAG) by specifying the dependencies of each task. This can be simpler to maintain for complex workflows and allows for maximum parallelism when running tasks.

In the following workflow, step A runs first, as it has no dependencies. Once A has finished, steps B and C run in parallel. Finally, once B and C have completed, step D can run.

```bash
argo submit -n argo --watch dag-diamond.yaml
```

## Verify Argo Workflows - DAG

Verify the change in the GUI.

![alt text](https://github.com/kartral21/argopoc/blob/master/dag.png?raw=true)

