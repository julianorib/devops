Neste artigo faremos uma explicação do comando top em sistemas Linux de forma detalhada, com o intuito de explicar e discutir cada opção apresentada por esta ferramenta tão importante para monitoração de sistemas Linux. O comando top é muito simples e existe e é também utilizado no macOS. No entanto, neste artigo, vamos explicar o uso do comando top apenas no Linux.


**top**


Opções úteis para visualização:
```
 1         --> mostra todos os processadores
 c         --> mostra os processos completo
 sft + T --> organiza por Tempo 
 sft + P --> organiza por Cpu
sft + N --> organiza por PID
sft + M --> organiza por Memória
sft + R  --> inverte a ordem selecionada anteriormente.
 sft + w --> Salva as ordenações e personalizações feitas no Top para o usuário.
 
 t ---> muda a visibilidade de Cpu
 m ---> muda a visibilidade de Memoria
```

Ver informações de CPU:
```
cat /proc/cpuinfo
```

Ver quantas CPU o Servidor tem:
```
cat /proc/cpuinfo | grep processor | wc -l
```

Explicação do comando top em sistemas Linux

```
top
```

#### Primeira linha do comando top no Linux**
<br>
Na primeira linha do comando top temos um resumo geral do sistema operacional, ou seja:
```
”HH:MM:SS”: Horário atual do sistema no formato horas, minutos e segundos.
```
”up” : Tempo que o sistema está ligado desde a última inicialização.

”user”: Quantidade de usuários conectados, ou via SSH (Secure SHell, que é o acesso via rede) ou via console (TTY – TeleTYpewriter).

”load average”: Carga média do sistema nos últimos 1, 5 e 15 minutos, respectivamente. Então basicamente é uma medida utilizada para calcular a média entre quantidade de processos em espera na fila de execução e CPU’s nos tempos determinados (1, 5 e 15 minutos).
Load average do comando top no Linux

Em primeiro lugar, fazendo uma analogia bem simples, vamos imaginar uma agência bancária onde temos um caixa de atendimento (1 CPU) e uma fila única (nossos processos). Portanto a quantidade de clientes que está sendo atendida ou que está na fila no período de 1, 5 e 15 minutos é o nosso load average.

Outro ponto interessante do load average é que sua interpretação é relativa: Ou seja, load average próximo de 0 pode significar que o sistema está ocioso, ou seja, recurso gastando energia elétrica à toa. Em contrapartida, se o load average está alto pode significar que o sistema está em uso e pode estar sobrecarregado ou dentro do esperado. Tudo depende de uma análise geral, portanto o load average pode ser um indicador, mas deve ser interpretado com outras medidas, tais como uso de CPU e quantidade de memória RAM utilizada.


mais informações sobre Load average:
https://pt.wikipedia.org/wiki/Load_average


#### Segunda linha do comando top no Linux – tasks
<br>
Na segunda linha temos um sumário da quantidade de tarefas ou processos com os seus respectivos status, por exemplo:


”total”: Total de tarefas no sistema

”running”: Total de tarefas que estão em processamento

”sleeping”: Total de tarefas em modo de espera. Estes processos estão em uso pelo sistema mas não estão em processamento

”stopped”: Total de tarefas paradas. São tarefas paralisadas ou com o atalho “Ctrl+c” durante uma execução no terminal ou utilizando o comando “kill -2 PID_do_processo” durante a execução de uma tarefa.

”zombie”: Total de tarefas em modo zumbi, que também são conhecidos como processo defunto (defunct). Portanto são processos que não existem mais no sistema operacional, mas por algum motivo (normalmente erros de software, processos finalizados de maneira abrupta) ainda permanecem na listagem dos processos.


#### Terceira linha do comando top no Linux – %CPU(s)


Na terceira linha temos informações de uso da CPU em percentagem. Visto que esta percentagem pode ser a média da quantidade que o seu sistema possui (sistemas com mais de 1 CPU).

”us”(user): É a percentagem de uso de CPU por processos de usuários

”sy ”(system): É a percentagem de uso de CPU por processos exclusivos do Kernel.

”ni”(nice): É a percentagem de uso de CPU de processos que tiveram sua prioridade modificada pelos comandos nice ou renice.

”id”(idle): É a percentagem de CPU que está livre para uso.

”wa”(wait): É a percentagem de uso de CPU em operações de entrada e saída, por exemplo leitura de disco.

”hi”(hardware interrupt): É a percentagem de uso de CPU por interrupções de hardware. Portanto são interrupções físicas dos periféricos, tais como disco físico e placa de rede.

”si”(system interrupt): É a percentagem de uso de CPU utilizada por interrupções de software. Portanto são interrupções de softwares que ocorrem diretamente para o Kernel.

”st”(steal): Esta percentagem de uso só funciona em ambientes de máquinas virtuais. Então quando uma máquina Linux virtual está rodando em cima de um hospedeiro físico (hypervisor), a percentagem mostra quanto tempo a CPU virtual está gastando esperando disponibilização da CPU do hospedeiro físico.


#### Quarta linha do comando top no Linux – KiB Mem


A quarta linha está relacionada a quantidade de memória RAM utilizada no sistema operacional, assim também os valores estão definidos em KiB (Kibibyte). Em outras palavras, Kibi (1024) é a medida binária do Quilo (1000) que é uma medida decimal. Só para exemplificar, no link abaixo temos uma explicação sobre a diferença entre Kibi e Quilo.

”total”: Total de memória RAM no sistema.

”free”: Total de memória RAM disponível no sistema.

”used”: Total de memória RAM em uso por processos no sistema.

”buff/cached”: Espaço de buffer ou cache são informações que foram lidas ou que precisam ser gravadas em disco, mas que ainda encontram-se na memória RAM.


#### Quinta linha do comando top no Linux – KiB Swap


A quinta linha está relacionada a quantidade de memória SWAP utilizada pelo sistema. Da mesma forma que a quarta linha, os valores estão em KiB (Kibibytes).

”total”: Total de memória SWAP no sistema.

“free”: Total de memória SWAP disponível no sistema.

“used”: Total de memória SWAP em uso pelo sistema

“avail Mem”: É uma estimativa de memória livre para inicialização de softwares, sem utilizar o swap

A saber, o avail Mem vêm do arquivo /proc/meminfo. Em suma, é a quantidade de memória livre que pode ser utilizado sem causar o uso de SWAP. No entanto cada distribuição e Kernel calcula esta “memória disponível” de forma diferente.


#### Sexta linha do comando top no Linux – Cabeçalho das colunas


Por fim, nas abas abaixo explicamos o que significa cada coluna da sexta linha apresentada pelo comando top:

“PID”: Process IDentification number – Todo processo recebe um número único pelo sistema operacional para identificá-lo.

“USER”: Nome do usuário dono do processo (owner).

“PR”: PRiority – Nível de prioridade de agendamento do processo. Assim sendo, esta prioridade é definida pelo próprio agendador do Kernel do sistema operacional. Além disso, o valor pode variar de -20 (maior prioridade) até 19 (menor prioridade).

“NI”: NIce – Valor definido pelos comandos nice ou renice. O valor padrão é 0.

“VIRT”: VIRTual (em kb): Quantidade de memória virtual em uso pelo processo
Portanto é o somatório de uso de memória SWAP com a memória resident (RES)
VIRT = SWAP + RES.

“RES”: RESident size (kb) – É o uso de memória física utilizada pela tarefa, sem considerar o SWAP

”SHR”: SHaRed memory – Quantidade de memória compartilhada com outros processos.

”S”: Status process – Estado do processo.

%CPU – Tempo da CPU utilizado pelo processo desde a sua última atualização.

%MEM de %MEMory – Percentagem de memória física utilizada pelo processo.

”COMMAND” – Nome do comando ou linha de comando com as opções adicionais. Ele aponta qual comando deu origem ao processo.


Estado do processo pelo comando top no Linux


Estado do processo pelo comando top pode ser:

D de Disk sleep – Também conhecido como “Uninterruptible sleep”. É um status especial onde o processo está aguardando para ser gravado em disco (i/o wait).
R de Running – Em processamento.
S de Sleeping – Aguardando processamento.
T de Traced or sTopped – Processo parado ou controlado pelo usuário.
Z de Zumbie – Processo em modo zumbi.


#### Comandos

Finalizar todos os processos de um usuário especifico:

```
kill -9 `ps -fu USER | awk '{print $2}' | grep -v PID`
```

Saída do comando TOP sem atualização em tempo real
<br>
Mostra a saida somente 1 vez:

```
top -n 1 -b
```

Mostra somente a primeira linha sem o cabeçalho:
```
top -n 1 -b | sed -n 8p
```

Mostra somente os 5 processos que estão utilizando mais memória:
```
ps aux | sort -nrk 3 | head -n 5 | awk '{print $3 " " $11}'
```

Mostra somente os 5 processos que estão utilizando mais CPU:
```
ps aux | sort -nrk 4 | head -n 5 | awk '{print $4 " " $11}'
```