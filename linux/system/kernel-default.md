# Alterar o Kernel padrão do Linux

Visualizar o Kernel Padrão atual.
```bash
grubby --default-kernel

uname -r
``` 

Verificar quais Kernels estão instalados e disponíveis.
```bash
grubby --info=ALL | grep ^kernel
```

Exemplo de saída:
```
kernel="/boot/vmlinuz-5.15.0-103.114.4.el9uek.x86_64"
kernel="/boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64"
```

Alterar o Kernel padrão para outro.
```bash
grubby --set-default "caminho/do/kernel"
grubby --set default /boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64
```

Executar uma reinicialização completa para verificar se está tudo ok.
```bash
reboot
```


### Referências:
https://blogs.oracle.com/linux/post/changing-the-default-kernel-in-oracle-linux-its-as-simple-as-1-2-3
