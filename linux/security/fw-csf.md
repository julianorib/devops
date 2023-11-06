# Firewall CSF no Linux

Instalar o Firewall

```
apt install iptables
cd /usr/src
wget download.configserver.com/csf.tgz
tar zxf csf.tgz
cd csf
sh install.sh
```

No início do arquivo de configuração procure pela linha TESTING e modifique para como mostrado abaixo:

vim /etc/csf/csf.conf
```
TESTING= "0"
```

Salve e aplique as alterações, reiniciando o Firewall

```
csf -f
```

Modifique as portas de acesso conforme sua necessidade:

vim /etc/csf/csf.conf
```
TCP_IN = "20,21,22,25,53,80,110,143,443,465,587,993,995"

TCP_OUT = "20,21,22,25,53,80,110,113,443"

UDP_IN = "20,21,53"

UDP_OUT = "20,21,53,113,123,33434:33523"
```

Reiniciar:
```
csf -r
```

Desativar:
```
csf -x
```

Ativar:
```
csf -e
```