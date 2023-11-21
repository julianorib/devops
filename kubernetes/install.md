# Como instalar um Cluster Kubernetes

## Pré-requisitos
Um cluster Kubernetes que lida com o tráfego de produção deve ter no mínimo três nós.

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
Melhor especificar 10.244.0.0/16

--service-cidr string    Default: "10.96.0.0/12"
Use um intervalo alternativo de endereço IP para VIPs de serviço.

--apiserver-cert-extra-sans string
SANs (Nomes Alternativos de Entidade) extras opcionais a serem usados para o certificado de serviço do Servidor de API. Podem ser endereços IP e nomes DNS.

Exemplo:
```
kubeadm init --ignore-preflight-errors=NumCPU 	--control-plane-endpoint "144.22.199.188:6443" 				 --upload-certs --node-name "master" 	  --pod-network-cidr=172.20.0.0/16 --service-cidr=172.19.0.0/16 --apiserver-cert-extra-sans=144.22.199.188
```

### Node

Copiar os códigos do ControlPlane para incluir os Nodes ao Cluster.


### CNI (Container Network Interface)

É necessário instalar um CNI para o Cluster funcionar.

https://github.com/flannel-io/flannel


### Validar configurações

```
kubectl get pods --all-namespaces
kubectl get pods
```

### MetalLB (load balancer)

Este é um Componente do Cluster para Criar um Balanceador de Carga em um Cluster Local.

https://metallb.universe.tf/installation/


### Criar um Storage Class do tipo NFS em um Cluster Kubernetes local.

https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner

#### Configuração Servidor NFS

Instalar um Servidor de NFS 
```
yum install nfs-utils
```

Criar uma pasta para compartilhar
```
mkdir /nfs
```

Ajustar permissões para a pasta criada.
```
chmod -R 777 /nfs

Criar as regras que definem quem poderá acessar este compartilhamento
```
vim /etc/exports
```
/nfs IpHost01(rw,sync,no_subtree_check,no_root_squash)
/nfs IpHost02(rw,sync,no_subtree_check,no_root_squash)
/nfs IpHost03(rw,sync,no_subtree_check,no_root_squash)
```

Reiniciar e Ativar o Serviço NFS-Server
```
systemctl enable nfs-server
systemctl restart nfs-server
```

#### Configuração Kubernetes

Requisito: \
Tenha o helm instalado.

Adicionar o repositório do nfs-subdir-external-provisioner
```
$ helm repo add nfs-subdir-external-provisioner https://kubernetes-sigs.github.io/nfs-subdir-external-provisioner/
```

Instalar e setar as configurações. \
Atenção: Altere os valores conforme necessário.
```
$ helm install nfs-subdir-external-provisioner nfs-subdir-external-provisioner/nfs-subdir-external-provisioner \
    --set nfs.server=192.168.1.5 \
    --set nfs.path=/nfs \
    --set storageClass.reclaimPolicy=Retain
    --create-namespace -n nfs-provisioner
```


### Ingress Controller

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