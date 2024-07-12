# Kube Prometheus Stack (with Grafana)

https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack
https://github.com/prometheus-operator/kube-prometheus
https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/user-guides/getting-started.md#include-servicemonitors

```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

```
helm repo update
```
```
helm show values prometheus-community/kube-prometheus-stack
```
```
kubectl create namespace monitoring
```
```
helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack -n monitoring
helm update --install monitoring prometheus-community/kube-prometheus-stack -n monitoring 
```

Ingress para o Grafana
```
apiVersion: traefik.containo.us/v1alpha1
kind: IngressRoute
metadata:
  name: grafana-ingressroute
  namespace: monitoring
spec:
  entryPoints:
    - web
  routes:
  - kind: Rule
    match: Host(`grafana.seudominio.com`)
    services:
    - name: monitoring-grafana
      port: 80
```

Senha admin grafana: prom-operator


# Prometheus e Grafana puro

https://artifacthub.io


helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/prometheus -n monitoring --set server.service.type=LoadBalancer

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install grafana grafana/grafana -n monitoring --set service.type=LoadBalancer 

