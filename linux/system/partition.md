# Gerenciar Partições / Espaço Usado


ver devices (hds e cdrom)

```
blkid
```

```
cat /proc/mount
```

Montar CD
```
mount /dev/sr0 /media/cd
```

Forçar montagem de todas partições que estejam no arquivo /etc/fstab, caso configure uma nova partição:
```
mount -a
```

Ver informações de discos (HD e partição);
```
fdisk -l
```

Ver utilização de Partições:
```
df -h
df -h /partição
```

Visualizar utilização de diretórios.
```
ncdu -x /
```

```
du
du -hs /pasta
du -hs *
du -hs .[^.]*
```