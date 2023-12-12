# Prometheus

https://artifacthub.io/packages/helm/prometheus-community/prometheus

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm inspect values prometheus-community/prometheus > values.yaml

Procurar alguns itens para modificar
- alertmanager (false)
- ingress: (true)
- service (loadbalancer) ???

helm install [RELEASE_NAME] prometheus-community/prometheus

Para configurar no POD, é necessário configurar Annotations no Deployment do projeto:

metadata:
  annotations:
    prometheus.io/scrape: "true"
    prometheus.io/path: /metrics
    prometheus.io/port: "8080"


# Grafana



# Kube Prometheus

https://github.com/prometheus-operator/kube-prometheus


https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack




