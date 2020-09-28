# Lab 7: Services

## Prerequisites

Remove the existing jedi deployment in your namespace before starting this lab.

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

Create those two deployments as basis for your service definitions:

**07-jedi-deployment.yaml**

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

**07-yoda-deployment.yaml**

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

### Verification

Validate that both Kubernetes services have been created. Both services are within a private IP address range that cannot be reached from the public internet.
Make a note though of the node port - in this example 30879, yours will be different.

```bash
$ kubectl get svc
NAME       TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
jedi-svc   NodePort    172.21.96.12     <none>        80:30879/TCP   57s
yoda-svc   ClusterIP   172.21.235.139   <none>        80/TCP         48s
```

To access the jedi-svc we have to find out the public IP addresses of our worker nodes. Using `kubectl get nodes` gives us also only private ip address segments.

```bash
$ kubectl get nodes
NAME             STATUS   ROLES    AGE    VERSION
10.134.237.212   Ready    <none>   2d3h   v1.17.11+IKS
10.134.237.244   Ready    <none>   2d3h   v1.17.11+IKS
10.134.237.245   Ready    <none>   2d3h   v1.17.11+IKS
```

The worker nodes within IBM Cloud are attached to a public and a private VLAN. To get to the public IPs you have to use `ibmcloud ks worker ls --cluster clustername'.

```bash
$ ibmcloud ks worker ls --cluster iks-garage-dach
NAME             STATUS   ROLES    AGE    VERSION
ID                                                       Public IP         Private IP       Flavor               State    Status   Zone    Version
kube-btnneqaf0ve384q6rsm0-iksgarageda-default-00000171   159.122.100.82    10.134.237.244   b3c.4x16.encrypted   normal   Ready    fra02   1.17.11_1539
kube-btnneqaf0ve384q6rsm0-iksgarageda-default-000002fd   159.122.100.91    10.134.237.245   b3c.4x16.encrypted   normal   Ready    fra02   1.17.11_1539
kube-btnneqaf0ve384q6rsm0-iksgarageda-default-0000033b   158.177.255.212   10.134.237.212   b3c.4x16.encrypted   normal   Ready    fra02   1.17.11_1539
```

In this example you can choose between 159.122.100.82, 159.122.100.91 and 158.177.255.212, as the node port is exposed on all three worker nodes.

So finally e.g. `159.122.100.82:30879` should return an nginx welcome screen.
