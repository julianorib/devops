## Ansible Playbook

[https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html)

Os Ansible Playbooks oferecem um sistema repetível, reutilizável e simples de gerenciamento de configuração e implantação de várias máquinas, adequado para implantação de aplicativos complexos. Se você precisar executar uma tarefa com Ansible mais de uma vez, escreva um playbook (instruções).

A estrutura de um arquivo simples de playbook é:

playbook.yaml
```
- name: Escrever o nome da Playbook
  hosts: grupodehosts
  tasks:
    - name: Escrever o nome da Primeira Tarefa
      ansible.builtin.debug:
        msg: "Esse é meu primeiro Playbook"
```

## Playbook com Roles (funções):

[https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

As funções permitem carregar automaticamente vars, arquivos, tarefas, manipuladores e outros artefatos Ansible relacionados com base em uma estrutura de arquivo conhecida. Depois de agrupar seu conteúdo em funções, você poderá reutilizá-lo facilmente e compartilhá-lo com outros usuários.

Uma função Ansible possui uma estrutura de diretórios definida com oito diretórios padrão principais. Você deve incluir pelo menos um desses diretórios em cada função. Você pode omitir quaisquer diretórios que a função não usa. Por exemplo:

[!roles.png](roles.png)

playbook.yaml
```
- name: Escrever o nome da primeira Role
  hosts: all
  roles:
    - containerd    # nome da primeira role
    - kubernetes    # nome da segunda role

- name: Escrever o nome da segunda Role
  hosts: control
  roles:
    - control       # nome da terceira role

- name: Escrever o nome da terceira Role
  hosts: worker
  roles:
    - worker        # nome da quarta role
```
os demais playbooks (main.yaml) iniciam os arquivos da seguinte maneira:
<br>
*não necessita colocar o nome da playbook, em quais hosts serão aplicadas, e o parametro tasks.*
```
---
  - name: Escrever o nome da Primeira Tarefa
    ansible.builtin.debug:
      msg: "Esse é meu primeiro Playbook"
```

## Como executar um Playbook

```bash
ansible-playbook -i hosts.cfg playbook.yaml 

ansible-playbook -i hosts.cfg playbook.yaml -b -K
```

## Modulos de Playbooks

### Criar um arquivo:   
```
  - name: Criação do Arquivo de Configuração do Sysctl para o K8s
    ansible.builtin.file:
      path: /etc/sysctl.d/k8s.conf
      state: touch
      mode: u=rw,g=r,o=r
```
### Copiar um arquivo ou pasta:
```
    - name: Copiar o Bitdefender para o Temp do Servidor
      ansible.builtin.copy:
        src: /etc/ansible/bdsec/
        dest: /tmp/bdsec/
```
### Trocar uma Linha em um arquivo: Lineinfile
```
task:
    - name: Trocar uma linha
      ansible.builtin.lineinfile:
        state: present
        dest: /opt/arquivo.txt
        regexp: "^# Listen 80"
        line: "Listen 8080"
```
### Comentar uma Linha em um arquivo: Lineinfile
```
task: 
    - name: Comentar a linha
      ansible.builtin.lineinfile:
        state: present
        path: /opt/arquivo.txt
        regexp: ''(^# Listen 80,*)'
        line: '# \1'
```
### Inserir uma linha antes ou depois em um arquivo: lineinfile
```
task:
    - name: Adicionar uma linha após determinada linha
      ansible.builtin.lineinfile:
        path: /etc/yum.repos.d/docker-ce.repo
        state: present
        insertafter: '^name=Docker CE Stable'
        firstmatch: yes
        line: 'exclude=containerd.io'
```
### Criar um bloco de informações em um arquivo:
```
  - name: Configurar os Modulos do Kernel para o K8s
    ansible.builtin.blockinfile:
      path: /etc/modules-load.d/k8s.conf
      block: |
        overlay
        br_netfilter
        nf_nat
        xt_REDIRECT
        xt_owner
        iptable_nat
        iptable_mangle
        iptable_filter
```
### Register
```
  - name: Get Join Command
    ansible.builtin.shell: kubeadm token create --print-join-command
    register: token

  - name: Linha de comando para Incluir Nós no Cluster
    ansible.builtin.debug:
      msg: "{{ token.stdout_lines[0] }}"
```
### Adicionar uma Chave GPG
```
  - name: Adicionando Chave GPG 
    ansible.builtin.apt_key:
      url: https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key
      state: present
      keyring: /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
### Adicionar um Repositório  
```
  - name: Adicionando Repositorio do Kubernetes
    ansible.builtin.apt_repository:
      repo: "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /"
      state: present
      filename: kubernetes
```
### Atualizar um pacote
```
    - name: Atualizar o kernel
      ansible.builtin.yum:
        name:
          - kernel-uek
        state: latest
```
### Instalar um pacote
```
    - name: Instalar o Apache (httpd)
      ansible.builtin.yum:
        name:
          - httpd
        state: present

    - name: Instalar Containerd
      ansible.builtin.apt:
        name: containerd.io
        state: present
        update_cache: yes
```
### Executar um determinado comando
```
  - name: Visualizar Kubeconfig remoto
    ansible.builtin.shell: cat /etc/kubernetes/admin.conf

  - name: Desabiltar o Swap
    ansible.builtin.command: swapoff -a
```
### Reiniciar 
```
  - name: Reboot
    ansible.builtin.reboot: 
      reboot_timeout: 120
```
