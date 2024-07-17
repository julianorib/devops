# Criando cotas de Recursos para um namespace

Crie um manifesto com o conteúdo abaixo.
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "1"
    limits.memory: 1Gi
```

Aplique com:
```
kubectl apply -f manifest.yaml -n Example
```

