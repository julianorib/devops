# Tmux

O que é?

Os criadores do tmux o descrevem como um multiplicador de terminal, ou seja, dentro de uma janela do terminal, você pode abrir várias janelas com visualizações baseadas em painéis. Onde cada painel tem um terminal executando de maneira independente, isso permite que você tenha vários comandos e aplicativos executando, sem a necessidade de abrir várias janelas.

Além disso, o tmux mantém essas janelas e painéis em uma sessão. Você pode sair de uma sessão a qualquer momento, chamamos isso de desanexação. Uma característica que considero incrível é que a sessão fica ativa até você matar o servidor do tmux (quando você reinicia o computador, por exemplo ou executa um comando que mata o servidor). Isso é incrivelmente útil porque depois em qualquer momento você pode escolher exatamente aquela sessão e o tmux irá te deixar no mesmo ponto de quando você a deixou, simplesmente anexando a essa sessão.

 
## Utilização:

 
Nova sessão no Tmux
```bash
tmux new -s session_name
```

Listar sessões no Tmux

```bash
tmux ls
```

Abrir sessão ativa no Tmux
```bash
tmux a -t session_name
```

Atalhos:
```
Sair do Painel CTRL+B     D
Fechar Painel  CTRL+D
```

Lidar com Janela
```
Nova Janela         CTRL+B   C
Próxima Janela      CTRL+B   N
Listar janelas      CTRL+B   W
Renomear Janela     CTRL+B   ,
```

Lidar com Painéis
```
Dividir vertical    CTRL+B  %
Dividir horizontal  CTRL+B  “
Eliminar o painel   CTRL+B  X
Mostrar Nº painel   CTRL+B  Q
Alternar painéis    CTRL+B  SETAS
```