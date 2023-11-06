# Como configurar a rede no Linux de forma manual:

Verificar interfaces de Rede.

```
cat /proc/net/dev
```

```
ls /sys/class/net
```
```
ip link show
```

### Configurações de Rede Centos

- Comandos
```
ifconfig 

ip addr

ifdown eth0

ifconfig eth0 192.168.1.2 netmask 255.255.255.0

ifup eth0   

ifconfig eth0 up

route add default gw 192.168.1.1 eth0
```

- Arquivo de configuração
```
/etc/sysconnfig/network-scripts/ifcfg-eth0
```
```
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.2
PREFIX=24
GATEWAY=192.168.1.1
DNS1=1.1.1.1
DNS2=8.8.8.8
```

Reiniciar rede:
```
systemctl restart network
```

### Configurações de Rede Debian

- Comandos

```
ip addr

ifdown enp0s3

ifup enp0s3  

ip addr add 192.168.1.8/24 dev enp0s3

ip route add default via 192.168.1.1
```

- Arquivo de configuração

```
/etc/network/interfaces
```
```
auto enp0s3
allow-hotplug enp0s3
iface enp0s3 inet static
    address 192.168.1.2
    netmask 255.255.255.0
    gateway 192.168.1.1
```
   
Reiniciar rede:
```
systemctl restart networking
```