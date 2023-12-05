# Metrics Server

https://github.com/kubernetes-sigs/metrics-server

O Metrics Server coleta Metricas de CPU e Memória dos Containers (Pods) do Kubernetes.

Para instalar, o ideal é fazer download do arquivo de manifesto e acrescentar uma linha de argumento no Deployment.
```
--kubelet-insecure-tls
```

Em seguida, aplique o Manifesto.

```
kubectl apply -f metrics-server.yaml
```

Para visualizar o consumo de CPU e Memória de um POD:
```
kubectl top pod nomedopod
```
