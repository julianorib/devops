Abaixo segue diversos comandos para Gerenciar, Manipular, Pesquisar arquivos no Linux.


### Listar arquivos

```bash
ls
```

Ordenar por data de modificação:
```bash
ls -lht
```

Ordenar por tamanho:
```bash
ls -lhS
```
  
Ver arquivos ocultos
```bash
ls -la
```

```bash
Ver somente pastas ocultas:
ls -dl .*/
```

```bash
Contar arquivos em uma pasta:
ls -1 /var/named | wc -l
```

### Pesquisar arquivos

Pesquisar por arquivos grandes: (G = Gb , M = Mb)
```bash
find /pasta/ -type f -size +1G
```

Pesquisar somente arquivo.
```bash
find /pasta -type f  -name algumacoisa. 
```

Pesquisar somente diretorio
```bash
find /pasta  -type d  -name algumacoisa. 
```

Pesquisar por alguma extensão, ignorando algum arquivo: (teste.txt)
```bash
find /pasta -name arquivo.txt ! -name teste.txt
```
Pesquisar por alteração do arquivo: (mtime = dias, mmin = minutos)
```bash
find /pasta -mtime N
find /pasta -mmin N
```

Pesquisar a quantidade de subpastas (-maxdepth 1,2,3,5)
```bash
find /pasta  -maxdepth 1 -name "*.php"
```

Pesquisar por um arquivo e executar um comando para cada resultado:
```bash
find /pasta -name arquivo.txt -exec ls -lh {} \;
find /pasta -name arquivo.txt -exec chmod 777 {} \;
```

Pesquisar por arquivos de um determinado usuário:
```bash
find / -user usuario
```

Pesquisar por um IP dentro de vários arquivos com o mesmo nome em pastas diferentes:
```bash
find /home/ -name .lastlogin -exec grep -Hin "1.2.3.4" {} \;
```


### Compressão de arquivos:

Compactar:
```bash
tar -czf projetos.tar.gz Projetos/
```

Descompactar arquivos:
```bash
tar -xzf projetos.tar.gz  
tar -xvf  aarquivo.tar
```

Arquivo tzst, deve instalar zst
```bash
yum install zstd
```

```bash
unzstd arquivo.tzst
```


### Visualizar em tempo real modificações nos arquivos: (logs em geral)

```bash
tail -f arquivo.log
```

Visualizar Ultimas 2 linhas somente:
```bash
tail -n 2 -f arquivo.log
```

Filtrando as linhas que deseja visualizar:
```bash
tail -f arquivo.log | grep 'filtro'
tail -f arquivo.log | egrep (filtro1|filtro2)
tail -f arquivo.log | egrep "filtro1|filtro2|filtro3"
```

Filtrando um comando para mostrar somente uma linha especifica.
```bash
comando | sed -n '2p' 
```


### Pesquisar conteúdo dentro de um ou vários arquivos:

Filtra a palavra teste em qualquer parte do arquivo teste.txt
```bash
grep "teste" /home/usuario/teste.txt
```

Filtra qualquer coisa, exceto teste dentro do arquivo teste.txt
```bash
grep -v "teste" /home/usuario/teste.txt
```

Filtra teste ignorando Maiuscula/Minuscula no arquivo teste.txt
```bash
grep -i "teste" /home/usuario/teste.txt
```

Filtra somente linhas que começem com teste no arquivo teste.txt
```bash
grep ^teste /home/usuario/teste.txt
```

Filtra somente linhas que terminem com teste no arquivo teste.txt
```bash
grep teste$ /home/usuario/teste.txt
```

Filtra o conteúdo do arquivo, mas só exibe o nome do arquivo e não o conteúdo.
```bash
grep -l testetextoaarquivo /home/usuario/teste.txt 
```

Filtra a data do dia em um arquivo de log. Isto se a data estiver no formato 2022-12-25
```bash
grep $(date +%Y-%m-%d) /var/log/arquivo.log
```

Filtra uma palavra (foo) e mostra as 3 linhas acima da palavra correspondente e as 2 linhas abaixo da palavra correspondente.
```bash
grep -B 3 -A 2 foo README.txt
```

Filtra uma palavra (foo) e mostra as 3 linhas acima e abaixo da palavra correspondente.
grep -C 3 foo README.txt

**Todos estes filtros funcionam combinando com o PIPE |**

### Substituir um caracter por outro

Este comando permite substituir um caracter por outro em um texto ou em uma saída filtrada.

```bash
tr ":" "\n"
echo $PATH | tr ":" "\n"
```


### Ver arquivos em uso por usuário, processo, etc.

```bash
lsof
lsof -u usuario
lsof -p PID
lsof -i:porta (80)
lsof -t /pasta
```


### Contar quantidade de linhas de saída de um comando ou linhas em um arquivo:

```bash
comando | wc -l
```
```bash
ls -lh | wc -l
```
```bash
cat arquivo.txt | wc -l
```
```bash
wc -l [ARQUIVO]
```

### Substituição de palavras

Substituir uma linha em um arquivo sem entrar dentro dele:
```bash
sed -i "s/TESTE=12345/TESTE=67890/g" arquivo.txt
```
substitui TESTE=12345 por TESTE=67890

**ATENÇÃO: sempre faça um backup dos arquivos antes.**


### Sincronização

Sincronizar arquivo mantendo as permissões:
```bash
rsync -avh --progress /origem/	/destino/
```