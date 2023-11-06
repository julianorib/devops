#rede #network #ping #Troubleshoot

Primeiramente antes de tudo, efetuar os seguintes testes em sequência.
1. Faça um teste de ping do Servidor para o Gateway / Firewall.  (protocolo ICMP)

```
ping 192.168.100.254
```

2. Faça um teste de ping do Servidor para a Internet via IP. (protocolo ICMP)

```
ping 8.8.8.8
```
```
ping 1.1.1.1
```

3. Faça um teste de ping do Servidor para a Internet via DNS. (procotolo ICMP / UDP)
```
ping google.com
```
```
ping uol.com.br
```

4. Faça uma verificação de resolver do DNS configurado. (protocolo UDP)
```
dig google.com
```
```
dig sicoob.com.br
```

5. Faça um teste de Roteamento do Servidor com algum local na Internet via IP. (protocolo ICMP)
```
traceroute -I 8.8.8.8
```
```
traceroute -I 1.1.1.1
```

6. Faça um teste de Roteamento do Servidor com algum local na Internet via DNS. (protocolo ICMP e UDP)
```
traceroute -I google.com
```
```
traceroute -I uol.com.br
```

7. Faça um teste de conexão do Servidor com algum local na Internet via DNS. (protocolo TCP)
```
telnet sicoob.com.br 80
```
```
telnet uol.com.br 80
```


Dica:  Utilize o tcpdump para monitorar o trafego.

```
 tcpdump -ni interface protocolo host ip
```
```
tcpdump -in ix1 udp host 192.168.100.254
```


 
### nslookup

É uma ferramenta comum ao Windows e ao Linux e utilizada para se obter informações sobre registros de DNS de um determinado domínio, host ou ip.

Exemplo de nslookup para o domínio WIKIPEDIA.ORG

```
C:\> nslookup wikipedia.org
```
```
Servidor:  server1.contoso.com
Address:  192.168.1.1

Nao é resposta autoritativa:
Nome:    wikipedia.org
Addresses:  2620:0:861:ed1a::1
          208.80.154.224
```

Em uma busca nslookup padrão, o servidor DNS do provedor de acesso é consultado, e retorna as informações sobre o domínio ou host pesquisado.


Exemplo de nslookup para o mesmo domínio WIKIPEDIA.ORG, agora realizando a consulta por um outro DNS (8.8.8.8).

```
C:\> nslookup wikipedia.org NS1.WIKIMEDIA.ORG
```
```
Servidor:  dns.google
Address:  8.8.8.8

Nao é resposta autoritativa:
Nome:    wikipedia.org
Addresses:  2620:0:861:ed1a::1
          208.80.154.224
```

**Opções mais comuns:**
Verificar informações de Responsável pelo domínio.
```
C:\> nslookup -type=SOA uol.com.br
```
```
Servidor:  server1.contoso.com
Address:  192.168.1.1

UOL.COM.BR
        primary name server = a10-dns-uolcsfe1.host.intranet
        responsible mail addr = root.UOL.COM.BR
        serial  = 2016053019
        refresh = 7200 (2 hours)
        retry   = 3600 (1 hour)
        expire  = 432000 (5 days)
        default TTL = 900 (15 mins)
```

Verificar registros de Email (mx) do domínio.
```
C:\> nslookup -type=MX uol.com.br
```
```
Servidor:  server1.contoso.com
Address:  192.168.1.1

Nao é resposta autoritativa:
UOL.COM.BR      MX preference = 10, mail exchanger = mx.UOL.COM.BR

mx.UOL.COM.BR   internet address = 200.147.41.231
```

Verificar informações dos Servidores de Nomes que podem atualizar registros no domínio.
```
C:\> nslookup -type=NS sicoob.com.br
```
```
Servidor:  server1.contoso.com
Address:  192.168.1.1

Nao é resposta autoritativa:
UOL.COM.BR      nameserver = borges.UOL.COM.BR
UOL.COM.BR      nameserver = eliot.UOL.COM.BR
UOL.COM.BR      nameserver = charles.UOL.COM.BR

borges.UOL.COM.BR       internet address = 172.64.53.68
eliot.UOL.COM.BR        internet address = 200.221.11.98
charles.UOL.COM.BR      internet address = 172.64.52.59
```
Verificar o hostname a partir de um IP
```
C:\> nslookup -type=PTR 200.147.41.231
```
```
Servidor:  server1.contoso.com
Address:  192.168.1.1

Nao é resposta autoritativa:
231.41.147.200.in-addr.arpa     name = 200-147-41-231.static.uol.com.br
```

### ip add

This command shows you the IP addresses on the server, along with their subnet masks, broadcast addresses, and interfaces. In this example, lo is the loopback address that has the server's localhost IP address, and eth0 is the interface for the server's routable IP address:

```
[root@server ~]# ip add | grep "inet "
```

```
inet 127.0.0.1/8 scope host lo
inet 172.16.1.7/24 brd 172.16.1.255 scope global eth0
```


### route -n

You can view the contents of a server's routing table with this command. You can use this command to show the server's default gateway, as well as to show routes for particular networks.


In this example, the server's routing table shows that the default gateway is 172.16.1.1 because it has the "0.0.0.0" entry under the Destination field. The table also shows the routes to the 172.16.1.0 network for the server's routable IP address, as well as a host route to the default DHCP address, 169.254.169.254:

```
[root@server ~]# route -n
```
```
Kernel IP routing table
Destination Gateway Genmask Flags Metric Ref Use Iface
169.254.169.254 172.16.1.1 255.255.255.255 UGH 0 0 0 eth0
172.16.1.0 0.0.0.0 255.255.255.0 U 0 0 0 eth0
0.0.0.0 172.16.1.1 0.0.0.0 UG 0 0 0 eth0
```


### traceroute

traceroute is another useful utility that shows the path packets take between your server and a remote server. You can use traceroute if your server is unable to reach a remote site, and the output shows you where the network packets are stopping.


In the example above, we used ping to test the connection to a remote site, example.org, that we could not reach. A traceroute to example.org shows that the packets are stopping after the router at IP 4.5.6.7 (Hop 12) so we need to contact the network administrator for that router:

```
[root@server ~]# traceroute example.org
```
```
traceroute to example.org (192.0.2.2), 64 hops max, 52 byte packets
 1 192.168.0.1 (192.168.0.1) 2.661 ms 1.558 ms 2.602 ms
 2 hostname3.example (1.2.1.50) 5.234 ms 3.985 ms 4.937 ms
 3 hostname4.example (1.2.1.60) 5.725 ms 4.344 ms 4.934 ms
 4 hostname5.example (1.2.1.90) 53.379 ms 55.082 ms 53.084 ms
 5 1.2.5.100 (1.2.5.100) 53.223 ms 51.678 ms 53.534 ms
 6 1.2.1.110 (1.2.1.110) 53.753 ms
 1.2.1.111 (1.2.1.111) 53.219 ms
 1.2.1.112 (1.2.1.112) 53.365 ms
 7 1.2.3.125 (1.2.3.125) 53.279 ms
 1.2.3.126 (1.2.3.126) 52.564 ms
 1.2.3.127 (1.2.3.127) 52.193 ms
 8 * * *
 9 2.3.4.1 (2.3.4.1) 108.640 ms
 2.3.4.3 (2.3.4.3) 106.246 ms *
 10 * 2.3.4.5 (2.3.4.5) 85.775 ms *
 11 * * *
 12 4.5.6.150 (4.5.6.150) 100.801 ms
 4.5.6.160 (4.5.6.160) 73.908 ms
 4.5.6.7 (4.5.6.7) 77.999 ms
 13 * * *
 14 * * *
 15 * * *
 16 * * *
 17 * * *
 18 * * *
 19 * * *
 20 * * *
```

No Linux, o traceroute utiliza o protocolo UDP echo requests, então pode ser barrado no Firewall.
<br>
Com a opção -I utiliza protocolo ICMP echo request.

```
[root@server ~]# traceroute -I 8.8.8.8
```
```
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  host51-2.viabrs.com.br (177.11.51.2)  0.266 ms  0.265 ms  0.312 ms
 2  192.168.91.3 (192.168.91.3)  0.396 ms  0.405 ms  0.403 ms
 3  as15169.saopaulo.sp.ix.br (187.16.218.58)  1.849 ms  1.853 ms  1.751 ms
 4  74.125.243.1 (74.125.243.1)  1.944 ms  1.988 ms  1.987 ms
 5  142.251.78.11 (142.251.78.11)  1.981 ms  1.983 ms  2.043 ms
 6  dns.google (8.8.8.8)  1.969 ms  1.891 ms  1.888 ms
```

### netstat

Exibe conexões TCP ativas, portas nas quais o computador está escutando, estatísticas ethernet, a tabela de roteamento ip, estatísticas IPv4 (para os protocolos IP, ICMP, TCP e UDP) e estatísticas IPv6 (para os protocolos IPv6, ICMPv6, TCP sobre IPv6 e UDP sobre IPv6). Usado sem parâmetros, esse comando exibe conexões TCP ativas.

Linux:
```
[root@server ~]# netstat -tuan
```
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:587           0.0.0.0:*               LISTEN
tcp        0      0 170.247.60.5:53         0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:53            0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:953           0.0.0.0:*               LISTEN
tcp        0      0 0.0.0.0:1951            0.0.0.0:*               LISTEN
tcp        0      0 127.0.0.1:3306          127.0.0.1:50028         TIME_WAIT
tcp        0      0 127.0.0.1:53244         127.0.0.1:25            TIME_WAIT
tcp        0      0 170.247.60.5:1951       177.11.49.250:63847     ESTABLISHED
tcp        0      0 127.0.0.1:25            127.0.0.1:53226         TIME_WAIT
tcp        0      0 127.0.0.1:3306          127.0.0.1:50030         TIME_WAIT
udp        0      0 170.247.60.5:53         0.0.0.0:*
udp        0      0 170.247.60.5:53         0.0.0.0:*
udp        0      0 127.0.0.1:53            0.0.0.0:*
udp        0      0 127.0.0.1:53            0.0.0.0:*
udp        0      0 127.0.0.53:53           0.0.0.0:*
```
mais informações (linux):
https://www.linuxforce.com.br/comandos-linux/comandos-linux-comando-netstat/

Windows:
```
C:\> netstat -an
```
mais informações (windows):
https://docs.microsoft.com/pt-br/windows-server/administration/windows-commands/netstat

**Complemento:**
```
-a        Exibe todas as conexões
-t        Protocolo TCP
-u        Protocolo UDP

-e        Mostra estatisticas sobre a conexão (pacotes)
-n        Exibição númerica de números de Portas e IP

-c        Modo continuo
-l        Portas de Escuta
```

Mostra as Portas TCP/UDP de Escuta ativas:
```
netstat -lntu
```
Mostra as Conexões ativas de um determinado IP:

```
netstat -an | grep IP
```
Mostra as Conexões ativas de uma Porta:
```
netstat -an | grep :Port
```
```
netstat -an | grep -w Port
```

Catalogando a quantidade de conexões estabelecidas por IP:

```
netstat -tn 2>/dev/null | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
```

Catalogando a quantidade de conexões estabelecidadas por Porta:
```
netstat -tn 2>/dev/null | grep 443 | awk '{print $5}' | cut -d: -f1 | sort | uniq -c
```


### telnet

Testar a conexão com uma Porta para ver se a mesma está aceitando conexões.

```
[root@server ~]# telnet example.com 25
```

```
Trying 192.0.2.1...
Connected to example.com.
Escape character is '^]'.
```


### tcpdump

tcpdump is a utility you can use to monitor network packets and show data between two servers. In this example, start a ping session to example.com, and run tcpdump on the connection. The output shows ping sending ICMP requests and responses between the source and destination servers:

```
[root@server ~]# ping example.com > /dev/null &
[1] 15993
```
```
[root@server ~]# tcpdump -n host example.com
```
```
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on eth0, link-type EN10MB (Ethernet), capture size 65535 bytes
01:18:21.608436 IP 172.16.1.7 > 192.0.2.1: ICMP echo request, id 31038, seq 37, length 64
01:18:21.609635 IP 192.0.2.1 > 172.16.1.7: ICMP echo reply, id 31038, seq 37, length 64
01:18:22.609907 IP 172.16.1.7 > 192.0.2.1: ICMP echo request, id 31038, seq 38, length 64
01:18:22.610901 IP 192.0.2.1 > 172.16.1.7: ICMP echo reply, id 31038, seq 38, length 64
01:18:23.611168 IP 172.16.1.7 > 192.0.2.1: ICMP echo request, id 31038, seq 39, length 64
01:18:23.613632 IP 192.0.2.1 > 172.16.1.7: ICMP echo reply, id 31038, seq 39, length 64
01:18:24.612868 IP 172.16.1.7 > 192.0.2.1: ICMP echo request, id 31038, seq 40, length 64
01:18:24.614417 IP 192.0.2.1 > 172.16.1.7: ICMP echo reply, id 31038, seq 40, length 64
01:18:25.614681 IP 172.16.1.7 > 192.0.2.1: ICMP echo request, id 31038, seq 41, length 64
01:18:25.615495 IP 192.0.2.1 > 172.16.1.7: ICMP echo reply, id 31038, seq 41, length 64
^C
10 packets captured
10 packets received by filter
0 packets dropped by kernel
```

Capturar Pacotes de uma Interface de Rede:
```
tcpdump -i eth0
```
Capturar Pacotes na Interface de Rede eth0 no Protocolo TCP:
```
tcpdump -i eth0 tcp
```

Capturar Pacotes na Interface de Rede eth0 na Porta 22:

```
tcpdump -i eth0 port 22
```
Capiturar Pacotes na Porta 123 e com Protocolo UDP.
```
tcpdump udp port 123
```

Capturar Pacotes de uma determinada Origem:
```
tcpdump -i eth0 src 192.168.0.2
```

Capturar Pacotes de um determinado Destino:
```
tcpdump -i eth0 dst 192.168.0.2
```

### iftop

Monitorar Trafego de Rede Linux
```
 Press H or ? for help 4Mb                407Mb                611Mb                814Mb           0.99Gb
mqqqqqqqqqqqqqqqqqqqqvqqqqqqqqqqqqqqqqqqqqvqqqqqqqqqqqqqqqqqqqqvqqqqqqqqqqqqqqqqqqqqvqqqqqqqqqqqqqqqqqqqqq
51.222.1.1                              => 153.35.192.50                              0b      0b      0b
                                        <=                                         34.0Mb  32.4Mb  31.9Mb
198.27.121.115                          => 177.11.48.138                           62.0Kb  53.7Kb  21.2Kb
                                        <=                                         4.18Mb  3.34Mb  1.20Mb
198.27.121.115                          => 142.44.223.2                               0b   11.6Kb  4.25Kb
                                        <=                                            0b    886Kb   224Kb
198.27.121.115                          => 177.53.143.93                              0b   8.43Kb  5.03Kb
                                        <=                                            0b    226Kb   279Kb
192.95.51.9                             => 104.47.22.202                            509Kb   102Kb  25.4Kb
                                        <=                                         21.9Kb  4.39Kb  1.10Kb
192.95.51.14                            => 104.47.22.202                            402Kb  80.4Kb  20.1Kb
                                        <=                                         20.8Kb  4.17Kb  1.04Kb
198.27.121.115                          => 177.11.55.68                            2.91Kb  49.3Kb  15.7Kb
                                        <=                                         2.99Kb  4.66Kb  4.49Kb
51.222.189.129                          => 213.186.33.99                           5.56Kb  22.0Kb  20.0Kb
                                        <=                                         8.09Kb  28.1Kb  25.2Kb
51.222.189.129                          => 52.20.150.120                           11.6Kb  12.1Kb  8.23Kb
                                        <=                                         3.91Kb  3.91Kb  3.36Kb
51.222.1.171                            => 200.159.39.124                             0b   13.0Kb  3.48Kb
                                        <=                                            0b    764b    742b
198.27.121.115                          => 168.181.49.123                          14.5Kb  9.70Kb  6.28Kb
                                        <=                                          960b    544b    293b
51.222.1.101                            => 131.100.210.30                          4.00Kb   820b    205b
                                        <=                                         22.1Kb  4.42Kb  1.11Kb
51.222.1.183                            => 131.100.210.32                          4.00Kb   820b    205b
                                        <=                                         22.1Kb  4.42Kb  1.10Kb
qqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqqq
TX:             cum:   21.7MB   peak:   19.9Mb                            rates:   1.02Mb   421Kb  1.77Mb
RX:                     254MB           42.0Mb                                     38.4Mb  37.0Mb  34.8Mb
TOTAL:                  275MB           51.4Mb                                     39.5Mb  37.4Mb  36.5Mb
```
**Opções: **

 Desabilita / Habilita Resolução de Nomes<br>
 n - toggle DNS host resolution         

 Mostra a Porta Origem / Destino<br>
 p - toggle port display
 
 Desabilita / Habilita Resolução de Portas<br>
 N - toggle service resolution          

 Esconde / Mostra coluna de Origem<br>
 s - toggle show source host            
 
 Esconde / Mostra coluna de Destino<br>
 d - toggle show destination host       
 
 Mostra o trafego em 2 linhas, em 1 linha, somente Recebidos ou somente Enviados.<br>
 t - cycle line display mode           
 
 Mostra acumulado de trafego.<br>
 T - toggle cumulative line totals

 P - pause display<br>
 q - quit


### nmap

Varredura de portas abertas / escuta em um determinado Host/IP:
```
root@mx01:/var/log# nmap localhost
```
```
Starting Nmap 7.60 ( https://nmap.org ) at 2022-08-05 16:48 -03
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000050s latency).
Not shown: 991 closed ports
PORT     STATE SERVICE
25/tcp   open  smtp
53/tcp   open  domain
80/tcp   open  http
443/tcp  open  https
783/tcp  open  spamassassin
2525/tcp open  ms-v-worlds
3306/tcp open  mysql
5000/tcp open  upnp
8080/tcp open  http-proxy
```

Para scanner em Portas UDP
```
root@mx01:/var/log# nmap -sU -v 1.2.3.4
```
```
Starting Nmap 7.60 ( https://nmap.org ) at 2022-08-05 16:48 -03
Nmap scan report for 1.2.3.4
Host is up (0.0000050s latency).
Not shown: 991 closed ports
PORT    STATE SERVICE
137/udp open  netbios-ns
```

### iptables

Bloqueia o acesso de IPs. (Firewall de linha de comando Linux).

Mostra todas as regras ativas:

```
root@jessie:/# iptables -L
```
```
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
​

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination


Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

```
Mostra regras de Entrada (Tabela INPUT)

```
iptables -L INPUT
```

Mostra regras de Saída (Tabela OUTPUT)
```
iptables -L OUTPUT
```
Mostra detalhes da regra de Entrada.
```
iptables -L INPUT -v
```

Bloqueando um IP
```
iptables -I INPUT -s 1.2.3.4 -j DROP
```

Bloqueando um IP em determinada interface de Rede.
```
iptables -A INPUT -i eth1 -s 1.2.3.4 -j DROP
```

Bloqueando uma Subrede
```
iptables -A INPUT -i eth1 -s 1.2.3.4/24 -j DROP
```

