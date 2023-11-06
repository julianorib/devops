Partição GPT

Extender o disco.
sgdisk -e /dev/sda

Remover e adicionar novamente a 3a partição (a que está o LVM)
sgdisk -d 3 /dev/sda
sgdisk -N 3 /dev/sda

partprobe /dev/sda


Lógica parecida com adicionar nova partição HotDisk.
A diferença é que só extende.

Extender a partição LVM:

pvresize /dev/sda3

vgdisplay | grep Free
lvdisplay | grep Path

lvextend -r -l+XXX /dev/xxxx/xxxx


