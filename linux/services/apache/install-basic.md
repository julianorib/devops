# Instalação Servidor Web

- Apache
- Mod_ssl
- Php
- Modulos Php


## Verificando em outro servidor configurações atuais e módulos:

```
httpd -V -> versão
httpd -S -> Virtualhost
php -version
```

## Instalando Pacotes

```
yum install httpd
yum install mod_ssl
yum module install php:7.2 (ou outra versão)
yum install php-pgsql
systemctl enable httpd
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/localhost.key -out /etc/pki/tls/certs/localhost.crt
apachectl configtest
yum restart httpd
```
