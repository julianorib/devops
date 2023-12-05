# Ingress Controller NGINX

Este é um Componente do Cluster para Criar um Proxy para expor diversos Serviços em uma mesma Porta, através de Subdomínio ou Path.

https://github.com/kubernetes/ingress-nginx

Baixe o Manifesto:
```
curl -O https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.0/deploy/static/provider/baremetal/deploy.yaml
```
Editar o manifesto. Pesquisar por NodePort e alterar para LoadBalancer.

Aplicar o manifesto.
```
kubectl apply -f deploy.yaml
```