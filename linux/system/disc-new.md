# Criar novo disco no Linux sem reboot

Adicione o novo disco pelo Vmware na VM em questão.

Execute o utilitário para criar a partição.
```
cfdisk /dev/sdb
```

Crie uma nova partição do tipo LVM e grave as alterações

Execute um partprobe.
```
partprobe /dev/sdb
```

Visualize as partições
```
cat /proc/partitions
```

Crie um novo Volume fisico (physical volume)
```
pvcreate /dev/sdb1
```
Visualize o Volume físico criado:
```
pvdisplay
```

Crie um novo Grupo de Volume (volume group)
```
vgcreate vg_dados /dev/sdb1
```
Visualize o novo Grupo de Volume criado:
```
vgdisplay
```

Visualize o valor da esquerda na linha Free PE do Grupo de Volume criado e anote este valor.

```
vgdisplay vg_dados | grep Free
```

Crie um novo Volume Lógico (logical volume) com o valor do "Free PE" no -l+2559
```
lvcreate -l+2559 vg_dados -n lv_dados
```
Visualize o novo Volume Lógico criado:
``` 
lvdisplay
```

Formate a partição:
```
mkfs.xfs /dev/vg_dados/lv_dados
mkfs.ext4 /dev/vg_dados/lv_dados

```

Monte a partição em algum local:
```
mkdir /dados
mount /dev/vg_dados/lv_dados /dados
```

Acrescente o ponto de montagem permanente no fstab.
```
vim /etc/fstab
```

Copie a linha referente ao ponto de montagem do /root e só altere os dados necessários.
```
/dev/mapper/vg_dados-lv_dados   /dados  xfs defaults    0   0
```

