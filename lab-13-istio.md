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

    kubectl get svc -n istio-system -w

Ensure the corresponding istio-* pods are all in *Running* state :

    kubectl get pods -n istio-system


## 1. Deploy the Guestbook sample app

The Guestbook app is a sample app for users to leave comments. It consists of a web front end, Redis master for storage, and a replicated set of Redis slaves. We will also integrate the app with Watson Tone Analyzer which detects the sentiment in users' comments and replies with emoticons.

### Tasks:

* Enable Istio sidecar injection **in your dev-*your initials* namespace**
* Create the Redis database **in your namespace**
* Install both v1 and v2 versions of the Guestbook app **in your namespace**
* Create a Watson Tone Analyzer named tone-analyzer-*your initials* in the workshop's resource group and region `eu-de`.<br />You might need to choose a `standard` plan (first 1000 API calls are free).
* Add the service credentials for the Watson Tone Analyzer to `analyzer-deployment.yaml`
* Deploy the analyzer pods and service **into your namespace**

### Supporting Information:

* [Exercise: Deploying Guestbook sample application](https://github.com/IBM/istio101/blob/master/workshop/exercise-3/README.md)
* You can change the current namespace with `kubectl config set-context --current --namespace=dev-my-initials`


## 2. Observe service telemetry

Managed Istio comes with Jaeger, Grafana, Prometheus and Kiali to visualize the microservices within your application. The following exercise shows you how to use these tools to monitor and debug your microservices application.

### Tasks:

* Open the guestbook service with its external IP in your web browser
* Add a emotional statements to your guestbook
* Explore the Jaeger, Grafana, Prometheus and Kiali dashboards

**Note: You do not need to configure Istio yourself or create Grafana & Kiali secrets!**

### Supporting Information:

* [Exercise: Observe service telemetry](https://github.com/IBM/istio101/blob/master/workshop/exercise-4/README.md)
* [Visualizing Metrics with Grafana](https://istio.io/latest/docs/tasks/observability/metrics/using-istio-dashboard/)
* [Logging with Mixer and Fluentd](https://istio.io/latest/docs/tasks/observability/mixer/logs/fluentd/)
* [Visualizing Your Mesh](https://istio.io/latest/docs/tasks/observability/kiali/)
* [Distributed Tracing](https://istio.io/latest/docs/tasks/observability/distributed-tracing/jaeger/)
