# Iptables Basico

## Limpa regras atuais
```
iptables -F
```

## Bloqueios
```
iptables -A INPUT -p tcp --tcp-flags ALL NONE -j DROP
iptables -A INPUT -p tcp ! --syn -m state --state NEW -j DROP
iptables -A INPUT -p tcp --tcp-flags ALL ALL -j DROP
```

## Interface loopback
```
iptables -A INPUT -i lo -j ACCEPT
```
  
## Liberacao SSH e Ping
``` 
iptables -A INPUT -p tcp -m tcp --dport 22 -j ACCEPT -s 172.31.128.0/24
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
```

## Conexoes Estabelecidas
```
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```
  
## Bloquear todo o resto
```
iptables -P OUTPUT ACCEPT
iptables -P INPUT DROP
```


