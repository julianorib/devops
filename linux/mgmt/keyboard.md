## Ubuntu - Configurar Teclado

Em determinadas situações, que houver a necessidade de acessar um Servidor Ubuntu pelo Console do Vmware, o teclado estará desconfigurado, sendo praticamente impossível de se fazer alguma coisa.

Para configurar o teclado, tente editar o arquivo:

```
vim /etc/default/keyboard
```

Troque o **br** para **us** na linha que contem essa informação.

Salve e feche o arquivo.

Execute o comando:

```
setupcon
```

Fim.