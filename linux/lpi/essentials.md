# Estudos Linux Essentials


## Estrutura de diretórios

/bin binários do sistema (essenciais)
/sbin binários exclusivos do root

/usr/bin outros binários do sistema (não essenciais)
/usr/sbin outros binários do sistema exclusivos do root


## Comandos

Volta para o ultimo diretório 
```
cd - 
```

Acessa o home do usuário atual
```
cd ~
```

Arquivos do tipo Backup (~).

Colocar um ~ no final do arquivo.
```
arquivo.txt~
```

Inverte a ordem da listagem do ls -r
ls -lhatr


Criar pastas dentro de outras pastas que não existem ainda.
mkdir -p pasta1/pasta2/pasta3/pasta4


Mostrar estrutura das pastas
tree

Mostra numero da linha 
cat -n

Elimina todas as linhas em branco
cat -s

Mostra numero da linha somente que nao sao em branco
cat -b

zcat  .gzip
bzcat .bz2

Inverte a visualização. 
Do fim para o inicio.
tac arquivo

Verifica se houve alteração na origem e só copia se tiver sido alterado.
Economiza tempo, pois não ficará sempre copiando tudo.
cp -uvR 


salvar data e hora alterado na placa mae
hwclock -systohc


Mostra as linhas numeradas de um arquivo
nl

Ordenação: (sort)
sort -r  inverte ordenação
sort -n  ordenação numérica
sort +1  ordenação pela 2a coluna
sort -t " "  ordenação a partir do delimitador " "
sort -t "A"  ordenação a partir do delimitador "A"
sort -k 1 	ordenação pela 1o coluna
sort -k 2   ordenação pelo 2o coluna
sort -t ":" -k 2  

Mostrar mensagens do Kernel
dmesg

dmesg -T (mostra com horarário comum)

Logar com um outro usuário do sistema que não tenha o Shell
su - usuario -s /bin/bash

Enumera uma lista
seq 10
seq 2 10
seq 2 2 10
seq -w 100

Atributos para proteção de arquivos:
(Operação não permitida)
chattr
lsattr

mostra os atributos
lsattr   
lsattr -d  

Atributos: 
Modo imutável (proteção)
(+i)  altera atributos do arquivo (nem o root consegue apagar)
chattr +i arquivo   

Modo apending
(+a)  só permite adicionar linhas em um arquivo com echo >> arquivo.
Não permite excluir conteúdo original do arquivo.
Não funciona com editores de arquivo.
chattr +a arquivo.  


Cortar / Formatar exibição de Colunas a partir de um caracter.
cut -d ":" -f 1
cut -d ":" -f 1-3
cut -d ":" -f 1,7

Verificar diferenças entre arquivos e salvar a diferença em outro arquivo
diff -ru file1.txt file2.txt  diff.txt

Juntar a diferença de um arquivo a um arquivo original
patch -p0 N   diff.txt
patch -p1 N   diff.txt


Visualizar conteúdo de arquivos ou tela com pause.
more
less
zless (arquivos .gz)

Visualizar onde está o manual de um comando.
whereis ls

Visualizar onde está o binário de um comando no S.o.
which ls

Mudar prioridade de processos (-20 a 19) sendo só para root.
Usuários comuns 0 a 19.
nice -n -20 processo
nice -n 0 processo

Muda prioridade de um processo em execução:
renice -n 10 -p PID


vmstat
vmstat 1

Se executar esse comando como usuário ou remotamente, não será mais necessário conectar no Servidor.
Será necessário reinicia-lo.
killall5


Partição GPT 
Primeira partição 512MB tipo Sistema EFI (formatar como VFAT)
mkfs.vfat

Segunda partição 512MB tipo Linux (formatar como EXT3)
mkfs.ext3  	formatar

Terceira partição SWAP. Também pode ser LVM e Criar um Logical Volume.
mkswap		formatar

Quarta Partição Maior Tamanho. LVM. ext4 ou xfs
mkfs.ext4	Formatar

Zerar Partição.
wipefs   	apagar  destruir

Migrar dados de partições
mountar umma nova partição temporária. 
mount /dev/sdb1 /tmp/var
cp -a /var  /tmp/var

Criar entradas no fstab.
mount -o remount /partiçao (remontagem)
ou Reboot   

options no fstab
Para não ficar atualizando tempo de acesso em arquivos e dirs.
Em servidores que tem muita gravação em disco, é recomendável ativar estas opções.
defaults,noatime,nodiratime,norelatime
noexec não permite execução de binários na partição.
nosuid, nodev
