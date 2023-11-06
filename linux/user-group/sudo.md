Permitir que um usuário comum possa executar comandos de root.
<br>
É o equivalente a "Elevação com Privilégios de Administrador" do Windows.


### Adicionar o usuário em questão a um grupo:

#### Centos

```
usermod -aG wheel usuario
```

#### Debian

```
usermod -aG sudo usuario
```
 
<br>

### Utilização
Logar com o usuário e digitar o comando com sudo: (ex)

```
$ sudo systemctl restart sshd
```

Será solicitado a senha do usuário. 
<br>
Logo em seguida será executado o comando que necessita de permissões root.

 Caso o comando sudo não exista, instale o sudo:

#### Centos

```
yum install sudo
```

#### Debian
```
apt install sudo
```