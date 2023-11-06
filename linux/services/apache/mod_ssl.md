Este tutorial tem como principio, orientar como configurar um modulo SSL com certificado autoassinado no Apache Linux.
<br>
Esta é uma configuração básica.


Instale o Modulo mod_ssl
```bash
yum install mod_ssl
```

Arquivo de configuração do SSL
```bash
/etc/httpd/conf.d/ssl.conf
```
Neste caminho do arquivo é possível apontar um outro certificado. 
<br>
Um certificado válido ou autoassinado pelo AD.
<br>
Neste exemplo será um autoassinado pelo próprio servidor local.
```bash
SSLCertificateFile /etc/pki/tls/certs/localhost.crt
SSLCertificateKeyFile /etc/pki/tls/private/localhost.key
```

Gerando um certificado autoassinado **localmente**.
```bash
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/localhost.key -out /etc/pki/tls/certs/localhost.crt
```

Testando a configuração:
```bash
apachectl configtest
```

Reiniciando Serviço Apache:
```bash
systemctl restart httpd
```
<br>

#### Observações
É interessante que seja criado um outro arquivo, pode ser copiado do ssl.conf com o nome do domínio a ser utilizado. Nele é aconselhável configurar um VirtualHost e definir outros caminhos do certificado, com o nome do dominio no nome de arquivo do certificado também.

Exemplo:
```
/etc/httpd/conf.d/teste_com_ssl.conf
/etc/pki/tls/private/teste_com.key
/etc/pki/tls/certs/teste_com.crt
```
<br>

#### Referências:

[https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html](https://httpd.apache.org/docs/2.4/ssl/ssl_howto.html)