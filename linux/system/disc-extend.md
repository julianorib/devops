# Extender disco LVM

Primeiramente, acesse o Servidor por SSH.

Digite o comando abaixo para ver o tamanho atual do Disco:

```bash
fdisk -l
```
```
Disco /dev/sda: 8 GiB, 8589934592 bytes, 16777216 setores
Modelo de disco: VBOX HARDDISK
Unidades: setor de 1 * 512 = 512 bytes
Tamanho de setor (lógico/físico): 512 bytes / 512 bytes
Tamanho E/S (mínimo/ótimo): 512 bytes / 512 bytes
Tipo de rótulo do disco: dos
Identificador do disco: 0x15d5619d

Dispositivo Inicializar  Início      Fim  Setores Tamanho Id Tipo
/dev/sda1   *              2048   999423   997376    487M 83 Linux
/dev/sda2               1001472 16775167 15773696    7,5G 8e Linux LVM
```
 

No Vmware, altere o tamanho do Disco do Servidor que deseja.

No Linux, faça o rescan do sistema para identificar o novo tamanho de disco.

```bash
ls /sys/class/scsi_device/
echo 1 > /sys/class/scsi_device/0\:0\:0\:0/device/rescan
```

Ver novo tamanho de disco:
```bash
fdisk -l
```
```
Disco /dev/sda: 16 GiB, 16589934592 bytes, 32777216 setores
Modelo de disco: VBOX HARDDISK
```

Criar uma nova partição primária do tipo 8e (Linux LVM) com o novo espaço não alocado do disco.
```bash
cfdisk /dev/sda
```
- Selecionar o espaço Free, enter, novo (New), escolher Primária, Escolher o tipo 8e, gravar (Write) e sair (Quit)

Após sair do utilitário cfdisk. Executar o comando:
```bash
partprobe /dev/sda
```
Verificar que deve haver uma nova partição (ex: /dev/sda3)
```bash
cat /proc/partitions
```
```
   8        0  104857600 sda
   8        1    1048576 sda1
   8        2   51379200 sda2
   8        3   52428800 sda3
  11        0    1048575 sr0
 253        0   99737600 dm-0
 253        1    4063232 dm-1
   7        0    2186240 loop0
```

Criar um novo volume de LVM com a partição:
```bash
pvcreate /dev/sda3
```
Ver o "Volume Group" a ser adicionado a partição e em seguida adicioná-la ao Grupo:
```bash
vgdisplay | grep Name
```
```
VG Name          centos_vg
```
Extender o volume
```bash
vgextend centos_vg /dev/sda3
```
```
Volume group "centos_vg" sucessfully extended
```

Verificar o nome do "Logical Volume" o qual será aumentado e o tamanho a ser aumentado:
```bash
lvdisplay | grep Path
```
```
 LV Path                /dev/centos_vg/root
 LV Path                /dev/centos_vg/swap_1
```

```bash
vgdisplay | grep Free
```
```
Free PE / Size 10239 / <8.00 gb
```

Extender o "Logical Volume" com o espaço livre no "Volume Group".
```bash
lvextend -r -l+10239 /dev/mapper/centos_vg-root
```


## Outra forma:

echo 1 > /sys/class/scsi_device/0\:0\:0\:0/device/rescan
fdisk -l

cfdisk /dev/sda
Extender a partição3

partprobe /dev/sda

pvresize /dev/sda3

vgdisplay | grep Free

lvextend -r -l+100%FREE /dev/mapper/ol-root