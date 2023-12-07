# Como instalar um Cluster Kubernetes

## Pré-requisitos

CPU     | 2 
Memória | 2
Swap    | OFF

## Pré-instalação (dependências)

Instalar um Container Runtime (ContainerD)

## Instalar os componentes do Kubernetes 

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

- Debian:
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#k8s-install-0

- RedHat: 
https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#k8s-install-1

## Criando um Cluster

O primeiro passo é criar o Cluster em 1 Control Plane.

### Cluster Init

https://kubernetes.io/pt-br/docs/reference/setup-tools/kubeadm/kubeadm-init/

Opções para inicialização de um cluster kubernetes

--ignore-preflight-errors=NumCPU
Ignorar verificação de quantidade mínima de CPUs.

--control-plane-endpoint string			
Especifique um endereço IP estável ou um nome DNS para o plano de controle.
Utilizado quando for usar mais de 1 Control Plane.
O ideal é utilizar um LB com DNS ou algo assim.

--upload-certs
Carregue certificados de plano de controle para o kubeadm-certs Secret.

--node-name string
Especificar o nome do Node

--pod-network-cidr string
Especifique o intervalo de endereços IP para a rede do pod. Se definido, o plano de controle alocará automaticamente CIDRs para cada nó.
Em teoria este é o padrão 10.244.0.0/16.\
Mas pode ser utilizado outro. Ex: 10.135.0.0/16.
*Fiz testes com CIDR diferente de /16 e não funcionou. Provavelmente algum problema no Flannel sem correção ainda.*

--service-cidr string    Default: "10.96.0.0/12"
Use um intervalo alternativo de endereço IP para VIPs de serviço.

--apiserver-cert-extra-sans string
SANs (Nomes Alternativos de Entidade) extras opcionais a serem usados para o certificado de serviço do Servidor de API. Podem ser endereços IP e nomes DNS.

Exemplo:
```
kubeadm init --ignore-preflight-errors=NumCPU 	--control-plane-endpoint "144.22.199.188:6443" 				 --upload-certs --node-name "master" 	  --pod-network-cidr=172.20.0.0/16 --service-cidr=172.19.0.0/16 --apiserver-cert-extra-sans=144.22.199.188
```

### Node

Copiar o TOKEN para incluir os Nodes ao Cluster.


### CNI (Container Network Interface)

É necessário instalar um CNI para o Cluster funcionar.

https://github.com/flannel-io/flannel

*Observação*:\
O flannel está com o --pod-network-cidr setado fixamente como 10.244.0.0/16.\
Se você especificou outro na criação do cluster, tem que baixar o arquivo e alterar manualmente.

### Validar configurações

```
kubectl get pods --all-namespaces
kubectl get pods
```

## Cluster Reset

kubeadm reset -f && rm -rf /etc/cni/net.d && iptables -F && rm -rf $HOME/.kube/config
