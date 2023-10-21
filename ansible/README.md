# Ansible

O que é ? 
<br>
É uma ferramenta de automação e gerência de configuração.

Para que serve?
<br>
Normalmente para automatizar a instalação e configuração de uma infraestrutura "já existente".

Principios
<br>
No Linux precisa apenas de SSH e python instalado nos servidores. Arquitetura descentralizada.
Ansible roda de qualquer lugar.

Porque?
<br>
Super fácil, portável, praticamente independente, não é necessário conhecer progrmação,
trabalha com formato testo, é livre e de código aberto.

Inventário
<br>
É um arquivo que descreve os servidores a serem acessados pelo Ansible e suas particularidades.
Possui vários formatos. (ini, yaml, json, etc)

Comandos Avulsos
<br>
Chamados de ah-hoc são bons pra fazerem pequenos ajustes e testes.
baseiam-se nos módulos

Módulos
<br>
Utilizamos módulos para indicar aquilo que queremos fazer:
copy, file, package, user, template, service


## Configuração para ignorar checagem de Host.

/etc/ansible/ansible.cfg
```
[defaults]
host_key_checking=False
interpreter_python=auto_legacy_silent
```

## Definindo um arquivo de Inventário.

/etc/ansible/hosts.cfg

```
[webserver]
192.168.0.5
192.168.0.6
webserver01

[email]
sendmail

[bancodados]
192.168.0.8
192.168.0.9
bdprod.dominio.com

[clusterk8s:children]
controlplane
workers

[controlplane]
10.0.0.5
10.0.0.6

[workers]
10.0.0.7
10.0.0.8
10.0.0.9
```

## Group_vars 

Quando é utilizado sempre o mesmo Usuário e Chave SSH, é possível incluir estas informações em um arquivo e deixar como o padrão para a execução do ansible ou ansible-playbook.

Crie uma pasta:

/etc/ansible/
```
mkdir group_vars
```

Crie um arquivo:

/etc/ansible/group_vars
```
ansible_ssh_user: User
ansible_ssh_private_key_file: "/etc/ansible/keys/chave_rsa"
```

