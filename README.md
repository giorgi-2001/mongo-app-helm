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


Deploy with Argo CD:
```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

helm upgrade --install argocd argo/argo-cd -n argocd --create-namespace

kubectl apply -k argocd
```

This applies the Argo CD `Application` resources for Traefik, the app, and monitoring. Argo CD must already be installed in the cluster; these manifests do not install Argo CD itself.

The Argo CD applications also install Traefik as the Ingress controller. The Traefik chart is pinned to version `42.0.0`, which requires Kubernetes `1.30` or newer. The UI Ingress uses the `traefik` IngressClass. Traefik's `LoadBalancer` Service requires a cluster or cloud provider that supports external load balancers.