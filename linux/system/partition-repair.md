## Como reparar sistema de arquivos em modo de segurança RHEL

Inicie com um LiveCD

Para ver as partições
```
lsblk 
```

Se as partições forem em LVM, é necessário ativar os Volumes
```
vgchange -ay
```

Para visualizar os Volumes Logicos e Volume Groups
```
lvs

vgs
```

Reparar um Sistema de Arquivos ext4
```
e2fsck -fv /dev/sda3
e2fsck -fv /dev/mapper/particao
```


Reparar um Sistema de Arquivos xfs
```
xfs_repair -f /dev/sda3
xfs_repair -f /dev/mapper/particao
```

