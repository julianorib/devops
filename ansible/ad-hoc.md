# Comandos AD-HOC mais comuns

Sintaxe:

```
-i hosts.ini        - Especificar o arquivo com o Inventário de hosts
grupo               - Especificar o Grupo dentro do Inventário de Hosts
-u root             - Especificar o usuário que utilizar no destino
--private keyfile   - Especificar a Chave SSH associada ao usuário
-m ping             - Especificar o Módulo ping
-m shell            - Especificar o Módulo shell 
-a "ls"             - Quando o módulo necessita de argumentos. 
-b                  - Become Sudo - Se não for usuário root, mas deve executar com Elevação (sudo)
-K                  - Solicitar a senha do usuário que tem permissão ao Sudo.
```

Modulo de ping: Teste conexão
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m ping
```

Modulo Shell : execução de comandos do Linux
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m shell -a 'ls /root/'
ansible -i hosts.ini grupo -u usuario --private-key keys/chave -m shell -a "ls /root/"  
```
Modulo Copy: copiar arquivos
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m copy  -a "src=/pasta/arquivo dest=/pasta/arquivo"
```

Modulo User: Adicionar usuário
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m user  -a 'name=usuarionovo password="senhacriptogravada"'
```
Definir senha criptografada
```
mkpasswd --method=SHA-256
```

Modulo User: Remover usuário
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m user  -a 'name=usuarioexcluir state=absent'
```

Instalar Pacotes
Derivados Debian:
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m apt -a "name=zabbix-agent state=present"
```

Derivados Redhat:
```bash
ansible -i hosts.ini grupo -u root --private-key keys/chave -m yum -a "name=zabbix-agent state=present use_backend=yum"

ansible -i hosts.ini grupo -u root --private-key keys/chave -m dnf -a "name=zabbix-agent state=present"
```


Documentação Completa:
https://docs.ansible.com/
https://docs.ansible.com/ansible/2.3/intro_inventory.html
https://docs.ansible.com/ansible/2.8/user_guide/intro_adhoc.html
https://www.youtube.com/watch?v=9Lx6bCs4nJo