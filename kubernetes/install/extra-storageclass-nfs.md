# Criar um Storage Class do tipo NFS em um Cluster Kubernetes local.

https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner

## Configuração Servidor NFS

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

Deve-se instalar o nfs nos Servidores que serão os clientes.
```
yum install nfs-utils
yum install nfs-common
```

Para testar se está liberado a comunicação:
```
showmount -e Ip-Nfs-Server
```


## Configuração Kubernetes

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
    --set nfs.server=192.168.15.8 \
    --set nfs.path=/home/juliano/nfs \
    --set storageClass.reclaimPolicy=Retain \
    --create-namespace -n nfs-provisioner
```