# Novo Disco movendo Diretório existente

Este guia tem como objetivo instruir como adicionar um novo disco a um servidor, movendo uma pasta do disco principal para este novo disco. 
<br>
*Exemplo: /var*

Observação: Necessita a paralização do servidor (reiniciar).
Primeiramente, adicione um novo disco ao servidor pelo Vmware.

Acesse o servidor via ssh.

### Configuração da Partição
Execute o comando para atualizar o novo disco.
```
echo 1 > /sys/class/scsi_device/0\:0\:1\:0/device/rescan
```

Visualize se o novo disco aparece na consulta abaixo:
```
fdisk -l
```

Crie uma nova partição com o novo disco do tipo LVM (8e).
<br>
A partição provavelmente será a /dev/sdb1
```
cfdisk /dev/sdb
```

Execute o comando:
```
partprobe /dev/sdb
```

Crie um novo Volume fisico (Physical Volume).
```
pvcreate /dev/sdb1
```
Visualize os Grupos de Volumes existentes para não utilizar o mesmo nome ao criar um novo.
```
vgdisplay | grep Name
```

Crie um novo Grupo de Volume (Volume Group).
```
vgcreate vg /dev/sdb1
```

Visualize os Volumes Lógicos existentes para não utilizar o mesmo nome ao criar um novo.
```
lvdisplay | grep "LV Name"
```

Crie um novo Volume Lógico (Logical Volume) com todo o espaço disponível no Volume Lógico.
```
lvcreate -l+100%FREE vg -n var
```

Formate a partição.
```
mkfs.xfs /dev/mapper/vg-var
```

### Movendo partição existente para novo disco/partição

Reinicie o servidor em modo que não execute nenhum serviço.
<br>
Necessita da senha do root após reinicialização.
```
init 1
```

Crie uma pasta qualquer.
```
mkdir /mnt/newvar
```

Monte a partição criada anteriormente na pasta criada.
```
mount /dev/mapper/vg-var /mnt/newvar
```

Copie os dados da partição que deseja mover com todas as suas propriedades.
```
cp -apx /var/* /mnt/newvar
```

Apague a pasta que foi a origem da cópia.
<br>
**Lembre-se de fazer um backup antes**
```
rm -rf /var 
```
Crie a pasta de origem novamente.
```
mkdir /var
```

Adicione a partição e o ponto de montagem "/var" no fstab.
```
/dev/mapper/vg-var		/var	xfs		defaults	0	0
```

Reinicie o servidor.
```
reboot
```

Verifique o novo disco e ponto de pontagem com:
```
df -h
```