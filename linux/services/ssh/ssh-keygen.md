SSH sem senha e com chave

Acessar o Servidor via SSH e como usuário que deseja criar a chave.
Observação: O usuário deve ser o mesmo nos 2 locais (cliente e servidor).

```
$ ssh-keygen -t rsa
```

Digitar uma senha para a chave privada.

Será gerado 2 arquivos no home/.ssh/ do usuário. (id_rsa e id_rsa.pub)

Copiar o id_rsa.pub para o servidor que irá acessar sem senha (com chave) e adicionar o conteúdo do arquivo no .ssh/authorized_keys do usuário em questão.
tem 2 formas:

1a forma:
```
$ ssh-copy-id usuario@ipservidor
```

2a forma:
```
$ scp id_rsa.pub usuario@ipservidor:/tmp/
```

No servidor copiar o conteúdo do arquivo id_rsa.pub para o arquivo .ssh/authorized_keys
```
$ cat /tmp/id_rsa.pub >> /home/usuario/.ssh/authorized_keys
```

Permissões:
- permissão do arquivo authorized_keys (600)
- permissão da pasta .ssh (700)

Se der erro, verificar o dono da pasta .ssh
```
chown -R usuario:usuario /home/usuario/
```
 

Por padrão, o linux utiliza a chave que está dentro de /home/usuario/.ssh/id_rsa como chave do cliente para acessar o servidor.
Especificar uma chave em outro diretório ou chave diferente para conectar 
```
ssh -i nomechave servidor1.com
```
 