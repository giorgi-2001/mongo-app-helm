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
kubectl apply -k argocd -n argo-cd
```

This applies the Argo CD `Application` resources for the app and monitoring charts. Argo CD must already be installed in the cluster; these manifests do not install Argo CD itself.