# Helm Chart for Mongo App


Chart structure

mongo-app contains subcharts:
- mongo-app: restful api
- mongo-app-ui: frontend for the mongo app
- mongodb: database

monitoring containing subcharts:
- opentelemetry collector
- kube-prometheus-stack
- custom loki chart

Set up:
```bash
#!/bin/bash

helm dependency build monitoring
helm dependency build mongo-app

helm install monitoring ./monitoring -n monitoring --create-namespace
helm install mongo-app ./mongo-app -n mongo --create-namespace
```

Clean up:
```bash
#!/bin/bash

helm uninstall monitoring -n monitoring
helm uninstall mongo-app -n mongo

kubectl delete ns mongo monitoring
```

Deploy with Argo CD:
```bash
kubectl apply -k argocd
```

This applies the Argo CD `Application` resources for Traefik, the app, and monitoring. Argo CD must already be installed in the cluster; these manifests do not install Argo CD itself.

The Argo CD applications also install Traefik as the Ingress controller. The Traefik chart is pinned to version `42.0.0`, which requires Kubernetes `1.30` or newer. The UI Ingress uses the `traefik` IngressClass. Traefik's `LoadBalancer` Service requires a cluster or cloud provider that supports external load balancers.