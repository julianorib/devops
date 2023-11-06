## Vim não cola com o Mouse

Editar o arquivo de configuração do VIM:
```
vim /usr/share/vim/vim82/defaults.vim
```
encontrar as linhas e comentá-las com **"**

```
if has('mouse')
  if &term =~ 'xterm'
    set mouse=a
  else
    set mouse=nvi
  endif
endif
```

Salvar e sair.

Depois a opção de colar com o mouse funcionará normalmente.
