# GIT

O Git é um sistema de controle de versão gratuito e de código aberto criado por Linus Torvalds in 2005

## Conceitos

- Master
Principal Linha do tempo de um código.

- Branch
Linhas do Tempo de um código

- Merge
Unir linhas do tempo de código.

- Commit
Aplicar alterações de um código.

- Remote
Link do repositório.
Faz a ligação do repositorio local com o remoto.

- push
Atualiza o repositorio remoto a partir do local.

- pull
Atualiza o repositorio local a partir do remoto.

*Observação:*
arquivo readme.md é um arquivo de instruções.
Ele fica na pagina inicial do projeto com as instruções.


## Explicando os comandos: 

### Iniciando um novo repositorio
```
git init
```

### Configurar o git
- Para todos os usuarios
```
git config --system user.emal "email@dominio.com"
git config --system user.name "seu nome"
```

- Para o usuário atual
```
git config --global user.email "seuemail@dominio.com"
git config --global user.name "seu nome"
git config --global http.sslVerify false
```

- Para um projeto especifico (só funciona apos o git init)
```
git config --local user.email "seuemail@dominio.com"
git config --local user.name "seu nome"
```

### Gerando uma chave SSH para conectar com o Git
ssh-keygen -t rsa 

Defina um nome se quiser.
Copie o conteudo publico da chave (arquivo.pub) para a configuração do seu usuário no GITHub, GITLab, etc.


### Definir uma chave SSH para o repositorio
Colocar a linha abaixo no arquivo .git/config do projeto.
```
[core]
 sshCommand = "ssh -i ~/.ssh/id_rsa"
```

### Ignorando arquivos
Crie um arquivo .gitignore dentro do projeto.
Dentro do arquivo especifique um arquivo ou alguma extensão de arquivo.


### Subir o repositorio local para o Remoto. 
```
git remote add origin https://github.com/nomedousuario/nomeprojeto.git
git remote add origin git@github.com:usuario/projeto.git 
```

### Subir as alterações (commits) no Repositorio Remoto 
```
git push -u origin main (primeira vez)
git push 
```

### Adicionando arquivos ao projeto
```
git add nomedoarquivo
git add . (todos os arquivos)
```

### Verificando o status do projeto
```
git status
```

### Executando um commit no projeto
```
git commit -m "titulo do commit"
```

### Verificando commits executados
```
git log
```

### Ver commit de um arquivo especifico
```
git log -p nomedoarquivo
```

### Resetando um commit (um estado antigo do arquivo)
```
git checkout hashdocommit
git checkout main (volta para o ultimo commit da branch)
git reset --hard (altera para o commit que setou no checkout)
```

### Atualizar o Projeto local com os dados do Projeto Remoto
```
git pull
```

### Atualizar o Projeto Remoto com os dados do Projeto Local
```
git push
```

### Fazer o download completo do repositorio.
```
git clone git@github.com:usuario/projeto.git
```

## Branches
Linha do tempo atual / alternativas de um projeto

```
git branch *listar*
git branch nova_branch *criar*
git checkout nova_branch *altera*
git checkout -b nova_brach *cria uma nova e altera*
git branch -d nova_branch *apaga*
```

## Merge
Junta as alterações de alternativas / linhas do tempo
```
git checkout main
git merge nova_branch 
```

## Criando uma nova Branch e Subindo no Repositorio
```
git chekout -b nova_branch
git add novo-arquivo.txt
git commit -m "novo-arquivo.txt"
git push -u origin nova_branch
```

## Rebase
Junta as alterações de alternativas / linhas do tempo
Mas não é aconselhável fazer na Branch Main .
Somente em Dev Homol etc. 
Ela não mantem um historico correto dos Commits.

```
git checkout main
git rebase nova_branch
```

## Cherry Pick
Uma forma de pegar um Commit de uma Branch (não main) para outra Branch. Sem utilizar Merge ou Rebase.

```
git checkout branch_5
git log *para ver o hash do commit na branch que deseja capturar o conteudo*
git checkout branch_10
git cherry-pick  HASH 
```

## Fetch
Forma mais suave de atualizar o Repositório Local com o Remoto.
Quando esta em conflito alterações nos 2 locais e mesmo arquivo.

```
git fetch origin
git diff main origin/main
```

## Git LOG
Rastreabilidade
```
git log
git log arquivo.txt
git log --oneline *resumido*
git log -n 2  *2 ultimas alterações*
git log --graph *mostra as linhas de branchs*
git log --graph --oneline
git log --author ""
```