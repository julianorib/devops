# Script Backup com Limpeza

 

Script para geração de backups e remoção dos backups antigos.

Bem simples. Sem firula.

```
#!/bin/bash

#Variaveis

RETENCAO="10"
DESTINO="/backup"
DATA="$(date + '%d-%m-%Y')"
HORA="($date + '%H-%M')"


mkdir $DESTINO/$DATA

#Gerar Backup. 

tar cvf $DESTINO/$DATA/backup_$HORA.TXT /pasta/arquivos/

#Remover arquivos antigos

find $DESTINO/$DATA/* -type d -mtime +$RETENCAO -exec rm -r {} \;

Fim.
```