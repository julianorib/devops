Atenção. 
Normalmente precisa de diversas dependêcias de desenvolvimento:

1) Configurar o kernel

O arquivo README mostra várias opções para gerar o arquivo .config no diretório de trabalho. Este novo arquivo terá as configurações usadas na compilação do kernel.
Exemplos de como o arquivo .config pode ser gerado usando o utilitário make.

make config – usa modo texto.

make menuconfig – usa modo texto com cor, radiolists e diálogos (requer as bibliotecas ncurses).

make localmodconfig – cria um .config baseado na configuração atual e nos módulos carregados.

make oldconfig – cria um .config baseado na configuração atual do sistema (com todos os módulos, carregados ou não).

A opção “make menuconfig” é considerada a mais simples e fácil de usar, pois oferece um menu simples para navegar.

Para entrar em qualquer opção, use a tecla ENTER;
Para navegar nas opções, use as setas de direção;
Para selecionar, use a barra de espaços;
Para obter mais informações sobre cada opção é só pressionar as teclas Shift+?. Para sair, tecle ENTER.
Para cada módulo, pode-se escolher três opções.
built-in – o módulo é incorporado ao kernel na compilação (passa a fazer parte do código-fonte do kernel);
não habilitado – não é carregado na inicialização do sistema, mas está disponível quando for necessário;
habilitado – o módulo é carregado na inicialização do sistema (mas não faz parte do código-fonte do kernel).
Para sair, pressione a tecla ESC duas vezes seguidas.
Para obter as informações do hardware da máquina que usará o kernel, e assim definir corretamente a configuração, veja os arquivos do diretório /proc.

Se você apenas deseja criar um kernel para entender o processo de compilação, a maneira mais simples e rápida é usar a opção “make oldconfig” e dar Enter em todas as perguntas que são feitas (usa o padão atual do sistema).


Depois deve-se executar o comando make

make bzImage

make modules 

make install



