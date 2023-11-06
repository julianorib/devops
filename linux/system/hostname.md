Alterar Hostname sem reboot

1. Ajuste o novo nome no /etc/hosts:

1

vim /etc/hosts

 
2. Ajuste o nome no arquivo /etc/hostname, executando:

1

 echo "NOVO_NOME" > /etc/hostname


3. Ajuste o nome direto em memória (a grande sacada):

1

echo "NOVO_NOME" > /proc/sys/kernel/hostname