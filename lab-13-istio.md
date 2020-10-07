# Lab 13: Using Managed Istio with IBM Kubernetes Service

## Prerequisites

- [Istio CLI](https://istio.io/latest/docs/ops/diagnostic-tools/istioctl/)
- Access to IBM Cloud
- Kubernetes cluster
- Clone the lab repository:
```
git clone https://github.com/ibm/istio101
cd istio101/workshop
```

## 0. Enable Managed Istio

**Only FYI: Istio will already be enabled on your cluster**

Enable Managed Istio on your IBM Kubernetes Service cluster:

    ibmcloud ks cluster addon enable istio --cluster $MYCLUSTER

Ensure that the istio-* Kubernetes services are deployed. This might take up to 5 minutes:

    kubectl get svc -n istio-system

Ensure the corresponding istio-* pods are all in *Running* state :

    kubectl get pods -n istio-system


## 1. Deploying the Guestbook sample app

The Guestbook app is a sample app for users to leave comments. It consists of a web front end, Redis master for storage, and a replicated set of Redis slaves. We will also integrate the app with Watson Tone Analyzer which detects the sentiment in users' comments and replies with emoticons.

[Exercise: Deploying Guestbook sample application](https://github.com/IBM/istio101/blob/master/workshop/exercise-3/README.md)


## 2. Observe service telemetry

Managed Istio comes with Jaeger, Grafana, Prometheus and Kiali to visualize the microservices within your application. The following exercise shows you how to use these tools to monitor and debug your microservices application.

[Exercise: Observe service telemetry](https://github.com/IBM/istio101/blob/master/workshop/exercise-4/README.md)


## 3. Expose the service mesh with the Istio Ingress Gateway

The following exercise shows you how to use the Istio Ingress Gateway to expose microservices within your service mesh to external applications and users.

[Exercise: Expose the service mesh with the Istio Ingress Gateway](https://github.com/IBM/istio101/blob/master/workshop/exercise-5/README.md)

Open the hostname URL and share it in our Slack channel! 


## 4. Perform traffic management

This exercise demonstrates how you can manage and optimize the network traffic in your microservice application with Istio.

[Exercise: Perform traffic management](https://github.com/IBM/istio101/blob/master/workshop/exercise-6/README.md)


## 5. Secure your service mesh

In this exercise, you will secure your network in your Kubernetes-based microservices application with Istio.

[Exercise: Secure your service mesh](https://github.com/IBM/istio101/blob/master/workshop/exercise-7/README.md)
