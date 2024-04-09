# Upgrade Kubernetes Version

https://v1-28.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

1 - Atualize um nó do ControlPlane.
2 - Atualize nós adicionais do ControlPlane
3 - Atualize os nós de Trabalho (workers)

*Observações:*\
**Os certificados de serviços (APISERVER, ETC, CONTROLLER-MANAGER, KUBELET, ETC) são renovados automaticamente no Upgrade de versão.**

## Ajustar os repositórios

Verificar qual repositório está habilitado.\
Há repositórios por versão (1.28, 1.29, etc).
```
/etc/yum.repos.d/kubernetes.repo
```
## Upgrade kubeadm
```
yum list --showduplicates kubeadm --disableexcludes=kubernetes
```
**replace x in 1.28.x-* with the latest patch version**
```
yum install -y kubeadm-'1.28.x-*' --disableexcludes=kubernetes
```

## Plan the upgrade
```
kubeadm upgrade plan
```

## Upgrade the node 
*replace x with the patch version you picked for this upgrade*
```
kubeadm upgrade apply v1.28.x
```

Aguardar a mensagem:
```
[upgrade/successful] SUCCESS! Your cluster was upgraded to "v1.28.x". Enjoy!
[upgrade/kubelet] Now that your control plane is upgraded, please proceed with upgrading your kubelets if you haven't already done so.
```

## Upgrade demais nós Control Plane
```
kubeadm upgrade node
```

## Drain the node 
Prepare o nó para manutenção, marcando-o como não programável e removendo as cargas de trabalho:

*replace <node-to-drain> with the name of your node you are draining*
```
kubectl drain <node-to-drain> --ignore-daemonsets
```

## Upgrade kubelet & kubectl
```
yum list --showduplicates kubeadm --disableexcludes=kubernetes
```

*replace x in 1.28.x-* with the latest patch version*
```
yum install -y kubelet-'1.28.x-*' kubectl-'1.28.x-*' --disableexcludes=kubernetes
```

## Restart the kubelet 
```
systemctl daemon-reload
systemctl restart kubelet
```

## Undrain the node 

*replace <node-to-uncordon> with the name of your node*
```
kubectl uncordon <node-to-uncordon>
```

# Upgrade Nodes (Worker)

O procedimento de atualização em nós de trabalho deve ser executado um nó de cada vez ou alguns nós de cada vez, sem comprometer a capacidade mínima necessária para executar suas cargas de trabalho.

## Ajustar os repositórios

Verificar qual repositório está habilitado.\
Há repositórios por versão (1.28, 1.29, etc).
```
/etc/yum.repos.d/kubernetes.repo
```

## Upgrade Node (worker)

```
kubeadm upgrade node
```

## Drain the node 
Prepare o nó para manutenção, marcando-o como não programável e removendo as cargas de trabalho:

*replace <node-to-drain> with the name of your node you are draining*
```
kubectl drain <node-to-drain> --ignore-daemonsets
```

## Upgrade kubelet & kubectl

```
yum list --showduplicates kubelet --disableexcludes=kubernetes
```
*replace x in 1.28.x-* with the latest patch version*
```
yum install -y kubelet-'1.28.x-*' kubectl-'1.28.x-*' --disableexcludes=kubernetes
```

## Restart the kubelet 
```
systemctl daemon-reload
systemctl restart kubelet
```

## Undrain the node 

*replace <node-to-uncordon> with the name of your node*
```
kubectl uncordon <node-to-uncordon>
```

## Validando
```
kubectl get nodes
```

```
kubectl get pods -o wide
```