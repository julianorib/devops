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


## Extras (tools)

<https://artifacthub.io/>

### Metrics Server
<https://github.com/kubernetes-sigs/metrics-server>

### Metallb
<https://kind.sigs.k8s.io/docs/user/loadbalancer/>

### Nfs Storage Class
<https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner>

### Grafana
<https://grafana.com/docs/grafana/latest/setup-grafana/installation/helm/>

```
helm upgrade --install grafana grafana/grafana -n monitoring --create-namespace 
```
| Parametros | Descrição |
|---|---|
| --set persistence.enabled=true | Habilita a persistencia de volume |
| --set persistence.storageClassName=nfs-client | Apontamento para o StorageClass NFS |
| --set service.type=LoadBalancer | Cria o serviço como LoadBalancer |

Obtem a senha do Grafana:
```
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Dashs:
| ID | Description |
|---|---|
| 16966 | Container Log |
| 15757 | k8s-views-global |
| 15758 | k8s-views-namespace |
| 15759 | k8s-views-nodes |
| 15760 | k8s-views-nodes |


### Kube Prometheus Stack
<https://github.com/prometheus-operator/kube-prometheus>

```
helm upgrade --install prometheus prometheus-community/kube-prometheus-stack --create-namespace -n monitoring
```
| Parametros | Descrição |
|---|---|
| --set grafana.enabled=false | Não instalar o Grafana |
| --set prometheus.service.type=LoadBalancer | Cria o serviço como LoadBalancer |


### Prometheus Operator
<https://github.com/prometheus-operator/prometheus-operator/blob/main/Documentation/user-guides/getting-started.md#include-servicemonitors>

### Grafana Loki
<https://grafana.com/docs/loki/latest/setup/install/helm/>

```
helm install loki grafana/loki -n monitoring --values loki.yaml 
```
| Parametros | Descrição |
|---|---|
| --set singleBinary.persistence.storageClass=nfs-client | Apontamento para o StorageClass NFS |

Para criar um Datasources no Grafana:
```
URL: http://loki-gateway.namespace
HTTP Headers: X-Scope-OrgID : 1
```

### Promtail
<https://grafana.com/docs/loki/latest/send-data/promtail/installation/>


### Traefik
<https://doc.traefik.io/traefik/getting-started/install-traefik/#use-the-helm-chart>

```
helm upgrade --install traefik traefik/traefik --create-namespace -n traefik 
```
| Parametros | Descrição |
|---|---|
| --version 27.0.2  | Versão do Chart que contém o Traefik v2 |
--set image.tag="2.11.5" | Ultima versão do Traefik v2 |

Criando uma nova Porta Exposta
```
 --set ports.ssh.port=10022 --set ports.ssh.expose.default=true --set ports.ssh.exposedPort=10022 --set ports.ssh.protocol=TCP
```