# Lab 10: Using Sysdig

## Prerequisites

- Access to IBM Cloud
- Connected with Kubernetes cluster with attached Sysdig instance
- Curl or some equivalent tool to produce HTTP requests (e.g. postman or similar)

## Supporting Information

https://cloudnative101.dev/electives/monitoring/sysdig/
https://cloudnative101.dev/electives/monitoring/sysdig/activities/dashboards/
https://cloudnative101.dev/electives/monitoring/sysdig/activities/alerts/

## Challenges to be solved

Create the following test pod in your namespace, add a port mapping and run local curl requests to produce application metrics.

```bash
$ kubectl -n dev-"YOUR INITIALS" create deployment "YOUR INITIALS"-web-app --image=docker.io/kennethreitz/httpbin

$ kubectl -n dev-"YOUR INITIALS" create svc nodeport "YOUR INITIALS"-web-app --tcp=8080:80

$ kubectl -n dev-YOUR INITIALS port-forward service/my-web-app 8080:8080

$ while true; do sleep 1; curl http://localhost:8080/status/200 -si | head -1 ; done
```

It can take a few minutes until the metrics flow into your Sysdig instance for the first time.

1. Create a new Custom Dashboard based on the Kubernetes Service Golden Signal default templates called "'YOUR-INITIALS' Web Service - Golden Signals" with your namspace as scope and add it to your favorites

1. Create a new Custom Dashboard based on the Kubernetes Pod Overview default template called "'YOUR-INITIALS' Kubernetes Pod Overview" with your namespace as scope and add it to your favorites

1. Create an email notification channel called "'YOUR-INITIALS'-sysdig-channel"

1. Create a custom high severity metric alert with the following characteristics:

- Name: "['YOUR-INITIALS' Web Service] HTTP Errors"
- Metric: Sum of net.http.error.count
- Scope: Use kubernetes.namespace.name + kubernetes.service.name
- Trigger: if metric > 1 for the last 1 minute in average send a single alert
- Activate your created email notification channel for this alert

1. Create additional HTTP 500 errors with beyond additional curl requests

```bash
while true; do sleep 0.1; curl http://localhost:8080/status/500 -si | head -1 ; done
```

## Verification

You should receive alert emails from Sysdig.
