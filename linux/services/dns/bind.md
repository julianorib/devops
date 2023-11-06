# Servidor de DNS local Linux (Forwarders)

Quando um Servidor em especifico tiver um fluxo de consultas DNS muito grande, é aconselhável ter um Serviço de DNS local para armazenar um cache e estas consultas serem mais rápidas (por ser local e estar em cache) e por não ficar consultando o Servidor DNS remoto (excesso de consultas).


## Instalação para Debian / Ubuntu

Instalar os pacotes do Bind9

```
apt install bind9 bind9-doc dnsutils
```

### Observação para Ubuntu:

Remover o link do /etc/resolv.conf
```
unlink /etc/resolv.conf
```

Desabilitar o serviço do Resolved local.
```
service system-resolved stop
systemctl disable systemd-resolved
```

Verificar na pasta /etc/netplan/ deve ter um arquivo 01-network-algumacoisa.
Editar este arquivo e na sessão nameservers colocar conforme exemplo abaixo.

```
vim /etc/netplan/01-networkalgumacoisa
```
```
nameservers:
     search: [gk2.net.br]
     addresses: [127.0.0.1, 1.1.1.1, 8.8.8.8]
```

Aplicar as configurações da placa de rede:

```
netplan apply
```

## Configuração Geral

Editar o arquivo /etc/bind/named.conf.options e deixa-lo como abaixo:

```
vim /etc/bind/named.conf.options
```

```
options {
        directory "/var/cache/bind";

        forwarders {
        170.247.163.254
        8.8.8.8;
        1.1.1.1;
         };

        dnssec-validation auto;

        listen-on { 127.0.0.1; };
       //listen-on { 127.0.0.1; 192..168.1.1/}; ** colocar essa linha somente se quiser colocar para rede interna. (ip do servidor dns)
        allow-query { internals; };
        allow-recursion { internals; };
        allow-transfer { none; };
        version none;

};

acl internals {
        127.0.0.0/8;
        192.168.1.0/24 ** colocar essa linha somente se quiser colocar para rede interna.
};

```

Testar as configurações. 

Se não aparecer nada é que está correto.
```
named-checkconf
```


Editar o /etc/resolv.conf e apontar para 127.0.0.1

```
vim /etc/resolv.conf
```
```
nameserver=127.0.0.1
```

Verificar o arquivo /etc/nsswitch.conf se tem uma entrada :

```
vim /etc/nsswitch.conf
```
```
hosts:        files    dns
```

Reiniciar o serviço dns
```
systemctl restart bind9
```
Testar com o nslookup 
```
nslookup example.com
```
```
Server:        127.0.0.1
Address:    127.0.0.1#53

Non-authoritative answer:
Name:    example.com
Address: 183.23.55.66
```

## Testes de Desempenho de Resposta:
```
dig @8.8.8.8 google.com | grep "Query time:"
```

Teste em loop de 15 

```
for run in {1..15}; do dig @8.8.8.8 google.com  ; done | grep "Query time:"
```