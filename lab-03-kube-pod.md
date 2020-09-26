# Lab 3: Kubernetes Pod Creation

## Prerequisites

You can retrieve metadata from your Kubernetes cluster.

```bash
$ kubectl get nodes
NAME             STATUS   ROLES    AGE     VERSION
10.134.237.212   Ready    <none>   3h18m   v1.17.11+IKS
10.134.237.244   Ready    <none>   3h18m   v1.17.11+IKS
10.134.237.245   Ready    <none>   3h19m   v1.17.11+IKS
```

## Supporting Information

https://cloudnative101.dev/lectures/kube-core-concepts/

## Challenge to be solved

Write a pod definition named yoda-service-pod.yml Then create a pod in the cluster using this definition to make sure it works.

The specifications of this pod are as follows:

- Use the bitnami/nginx container image.
- The container needs a containerPort of 80.
- Set the command to run as nginx
- Pass in the -g daemon off; -q args to run nginx in quiet mode.
- Create the pod in the web namespace.

### Verification

When you have completed this lab, use the following commands to validate your solution.

```bash
$ kubectl get pods -n web

$ kubectl describe pod nginx -n web
```
