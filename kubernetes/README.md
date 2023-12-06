# Kubernetes / K8S

Livro Online:

https://livro.descomplicandokubernetes.com.br/pt/

Documentação Oficial:

https://kubernetes.io/pt-br/docs/home/


## Conceitos

 - Nó ou Worker:

Cada Servidor de um Cluster

- Cluster:

Um conjunto de Nós que formam um Cluster

- Control Plane:

Os componentes do plano de controle tomam decisões globais sobre o cluster (por exemplo, agendamento), bem como detectam e respondem a eventos de cluster (por exemplo, iniciando um novo POD quando o campo de uma implantação não estiver satisfeito)

- POD:

Um container ou um conjunto de Containers com a mesma função

- kubeadm:

Tool para realizar o bootstrap do Cluster

- kubelet:

tool que roda em todos os nodes gerenciando os papéis, iniciando pods, containers e mantendo-os saudáveis

- kubectl:

tool para realizar a comunicação com o Cluster e executar todas as operações de orquestração.


## Cluster Kubernets Componentes

![kube_componentes](kubernetes-components.png)

https://kubernetes.io/docs/concepts/overview/components/

## Componentes Control Plane

- Kube-apiserver
- etcd
- kube-scheduler
- kube-controller-manager
- cloud-controller-manager

## Componentes do Node

- Kubelet
- Kube-proxy
- Container Runtime


## Certificações:

- CKA
- CKS
- CKD


## Comandos mais usados

Configurar uma Variável de ambiente com dados do config para conexão com o Cluster.
```
export KUBECONFIG="${KUBECONFIG}:./config"
```

https://kubernetes.io/docs/reference/kubectl/cheatsheet/


https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands


## Padrão de manifesto

```
apiVersion:
kind:
metadata:
  name: 
spec:
```

