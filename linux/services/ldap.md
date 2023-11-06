### Teste de conexão do Linux com LDAP no Windows
```
ldapsearch -x -H ldap://servidor -b "OU=Users,DC=contoso,DC=com" -D 'user@contoso.com' -W
```
<br>

### Teste de conexão do Linux com LDAP no Windows (SSL)
```
ldapsearch -x -H ldaps://servidor:636 -b "OU=Users,DC=contoso,DC=com" -D 'user@contoso.com' -W
```
**Observação:**
<br>
Para funcionar o SSL, o client do LDAP deve estar configurado com o certificado.

#### Debian

Procure o arquivo:
<br>
/etc/ldap/ldap.conf

Coloque no final do arquivo:
```
TLS_CACERT      /etc/ssl/certs/certificado.crt
TLS_REQCERT allow
```
Coloque o arquivo de certificado no caminho especificado.

#### Redhat

Procure o arquivo:
<br>
/etc/openldap/ldap.conf

Coloque no final do arquivo:
```
TLS_CACERT      /etc/ssl/certs/certificado.crt
TLS_REQCERT allow
```
Coloque o arquivo de certificado no caminho especificado.