# Lab 7: Services

## Prerequisites

## Supporting Information

https://cloudnative101.dev/lectures/kube-services-networking/

Hint: make sure you select **Show more** on the code snippets on above site to see all content.

## Challenges to be solved

We have a jedi-deployment and yoda-deployment that need to communicate with others. The jedi needs to talk to the world (outside the cluster), while yoda only needs to talk to jedi council (others in the cluster).

Examine the two deployments, and create two services that meet the following criteria.

**jedi-svc**

- The service name is jedi-svc.
- The service exposes the pod replicas managed by the deployment named jedi-deployment.
- The service listens on port 80 and its targetPort matches the port exposed by the pods.
- The service type is NodePort.

**yoda-svc**

- The service name is yoda-svc.
- The service exposes the pod replicas managed by the deployment named yoda-deployment.
- The service listens on port 80 and its targetPort matches the port exposed by the pods.
- The service type is ClusterIP.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jedi-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: jedi
  template:
    metadata:
      labels:
        app: jedi
    spec:
      containers:
        - image: bitnami/nginx:1.16.1
          name: c
          ports:
            - containerPort: 8080
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: yoda-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: yoda
  template:
    metadata:
      labels:
        app: yoda
    spec:
      containers:
        - image: bitnami/nginx:1.16.1
          name: c
          ports:
            - containerPort: 8080
```
