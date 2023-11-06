# Aumentar SWAP

Antes de começar a visualizar os Swaps ativos, você pode visualizar se existem swaps ativos:

```bash
sudo swapon --show
```

```
NAME      TYPE      SIZE USED PRIO
/dev/sda2 partition 1.0G   0B   -2
```

1. Criando um Arquivo Swap
```bash
fallocate -l 4G /swapfile
```
ou
```bash
dd if=/dev/zero of=/swapfile bs=1K count=4M status=progress
count (1K * 4M = 4 GiB)
```
 

2. Ajustando as permossões do Swap
```bash
chmod 600 /swapfile
```

3. Configurando o Arquivo de Swap
```bash
mkswap /swapfile
```

4. Ativando o Arquivo de Swap
```bash
swapon /swapfile
```

5. Visualizando o Swap
```bash
sudo free -h
```
```
total        used        free      shared  buff/cache   available
Mem:           488M        158M         83M        2.3M        246M        217M
Swap:          2.0G        506M        517M
```

6. Deixando o Swap persistente
```bash
echo "/swapfile swap swap defaults 0 0" >> /etc/fstab
```
