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
* You can change the current namespace with `kubectl config set-context --current --namespace=ggckad-s2`


## 2. Observe service telemetry

Managed Istio comes with Jaeger, Grafana, Prometheus and Kiali to visualize the microservices within your application. The following exercise shows you how to use these tools to monitor and debug your microservices application.

### Tasks:

* Install and setup Grafana, Prometheus, Kiali and Jaeger
* Open the guestbook service with its external IP in your web browser
* Add a couple of emotional statements
* Explore the Jaeger, Grafana, Prometheus and Kiali dashboards

### Supporting Information:

* [Exercise: Observe service telemetry](https://github.com/IBM/istio101/blob/master/workshop/exercise-4/README.md)
* [Visualizing Metrics with Grafana](https://istio.io/latest/docs/tasks/observability/metrics/using-istio-dashboard/)
* [Logging with Mixer and Fluentd](https://istio.io/latest/docs/tasks/observability/mixer/logs/fluentd/)
* [Visualizing Your Mesh](https://istio.io/latest/docs/tasks/observability/kiali/)
* [Distributed Tracing](https://istio.io/latest/docs/tasks/observability/distributed-tracing/jaeger/)


## 3. Perform traffic management

This exercise demonstrates how you can manage and optimize the network traffic in your microservice application with Istio.

### Tasks:

* Create a `DestinationRule` and `VirtualService` to route 100% of the traffic to the v1 guestbook app
* *Verify:* Reload the guestbook page multiple times in your browser – all traffic should go to v1
* Modify the rules so that only Firefox users will see the v2 guestbook app
* *Verify:* Open the guestbook page in Firefox and Chrome – only Firefox should show v2
* Route 80% of traffic to v1 and 20% of traffic to v2
* *Verify:* Reload the page multiple times – most traffic should go to v1
* Route all traffic to v2
* *Verify:* Reload the page multiple times – all traffic should go to v2

### Supporting Information:

* [Exercise: Perform traffic management](https://github.com/IBM/istio101/blob/master/workshop/exercise-6/README.md)
* [Traffic Management](https://istio.io/latest/docs/concepts/traffic-management/)
* [Direct encrypted traffic from IBM Cloud Kubernetes Service Ingress to Istio Ingress Gateway](https://istio.io/latest/blog/2020/alb-ingress-gateway-iks/)


## 4. Secure your service mesh

In this exercise, you will secure your network in your Kubernetes-based microservices application with Istio.

### Tasks:

* Enforce an authentication policy that requires mutual TLS – **only for your namespace dev-*your-initials*!**<br />
  *Note: There is a [bug](https://github.com/istio/istio.io/issues/7862) in istioctl that may not show mTLS in the `istioctl x describe` output*
* Modify guestbook and analyzer deployments to identify with service accounts
* Create a `AuthorizationPolicy` to disable all access to analyzer service
* *Verify:* Validate the rule in your browser - the guestbook v2 app should no longer work
* Modify the `AuthorizationPolicy` so only the Guestbook service can access the Analyzer service
* *Verify:* Validate that the v2 guestbook app works now in your browser

### Supporting Information:

* [Exercise: Secure your service mesh](https://github.com/IBM/istio101/blob/master/workshop/exercise-7/README.md)
* [Lock down to mutual TLS by namespace](https://istio.io/latest/docs/tasks/security/authentication/mtls-migration/#lock-down-to-mutual-tls-by-namespace)
* [Secure in-cluster traffic by enabling mTLS](https://cloud.ibm.com/docs/containers?topic=containers-istio-qs#mtls-qs)
* [Bug: strict mutual TLS output in istioctl may not be correct](https://github.com/istio/istio.io/issues/7862)


## 5. Optional: Expose the service mesh with the Istio Ingress Gateway

The following exercise shows you how to use the Istio Ingress Gateway to expose microservices within your service mesh to external applications and users.

### Tasks:

* Create the Istio Ingress Gateway in your namespace
* Create a Network Load Balancer and connect it to the Istio Ingress Gateway
* Share the hostname URL in our Slack channel! 

### Supporting Information:

* [Exercise: Expose the service mesh with the Istio Ingress Gateway](https://github.com/IBM/istio101/blob/master/workshop/exercise-5/README.md)
* [Ingress Gateways](https://istio.io/latest/docs/tasks/traffic-management/ingress/ingress-control/)
* [Configure Istio Ingress Gateway](https://istio.io/latest/docs/examples/microservices-istio/istio-ingress-gateway/)
