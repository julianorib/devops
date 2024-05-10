# Comandos AD-HOC mais comuns

#### Sintaxe:
```
-i hosts.cfg        - Especificar um arquivo com o Inventário de hosts
grupo               - Especificar um Grupo dentro do Inventário de Hosts
-u root             - Especificar o usuário que utilizar no destino
--private keyfile   - Especificar a Chave SSH associada ao usuário
-m ping             - Especificar o Módulo ping
-m shell            - Especificar o Módulo shell 
-a "ls"             - Quando o módulo necessita de argumentos. 
-b                  - Become - Se não for usuário root, mas deve executar com Elevação (sudo)
-K                  - Solicitar a senha do usuário que tem permissão ao Sudo.
```

#### Modulo de ping: Teste conexão
```bash
ansible grupo -m ping

ou completo

ansible -i caminho-hosts.cfg grupo -u user --private-key caminho-chave -m ping
```

#### Modulo Shell : execução de comandos do Linux
```bash
ansible grupo -m shell -a "ls /root"

ou completo

ansible -i caminho-hosts.cfg grupo -u user --private-key caminho-chave -m shell -a "ls /root/"  
```
#### Modulo Copy: copiar arquivos
```bash
ansible grupo -m copy -a "src=/pasta/arquivo dest=/pasta/arquivo"

ou completo

ansible -i caminho-hosts.cfg grupo -u user --private-key caminho-chave -m copy -a "src=/pasta/arquivo dest=/pasta/arquivo"
```

**Alguns comandos exigiram permissão de super-usuário (sudo).**\
**Acrescentar no final da linha: -b -K**\
**Será solicitado digitar a senha**

#### Modulo User: Adicionar usuário
Definir senha criptografada
```
mkpasswd --method=SHA-256
```

```bash
ansible grupo -m user -a 'name=usuarionovo password="senhacriptogravada"' -b -K

ou completo

ansible -i caminho-hosts.cfg grupo -u user --private-key caminho-chave -m user -a 'name=usuarionovo password="senhacriptogravada"' -b -K
```

#### Modulo User: Remover usuário
```bash
ansible grupo -m user -a 'name=usuarioexcluir state=absent' -b -K

ou completo

ansible -i caminho-hosts.cfg grupo -u user --private-key caminho-chave -m user -a 'name=usuarioexcluir state=absent' -b -K
```

#### Instalar Pacotes
Derivados Debian:
```bash
ansible grupo -m apt -a "name=zabbix-agent state=present" -b -K

ou completo

ansible -i hosts.ini grupo -u user --private-key keys/chave -m apt -a "name=zabbix-agent state=present" -b -K
```

Derivados Redhat:
```bash
ansible grupo -m yum -a "name=zabbix-agent state=present use_backend=yum" -b -K

ou completo

ansible -i caminho-hosts.cfg grupo -u user --private-key caminho-chave -m yum -a "name=zabbix-agent state=present use_backend=yum" -b -K
```
<br>

#### Documentação Completa:
<br>

[https://docs.ansible.com/](https://docs.ansible.com/)

[https://docs.ansible.com/ansible/2.3/intro_inventory.html](https://docs.ansible.com/ansible/2.3/intro_inventory.html)

[https://docs.ansible.com/ansible/2.8/user_guide/intro_adhoc.html](https://docs.ansible.com/ansible/2.8/user_guide/intro_adhoc.html)

[https://www.youtube.com/watch?v=9Lx6bCs4nJo](https://www.youtube.com/watch?v=9Lx6bCs4nJo)