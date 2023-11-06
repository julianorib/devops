#### Listar Serviços

```bash
systemctl
```
```bash
systemctl list-unit-files --type service --all
```
```bash
systemctl | grep running
```

#### Iniciar

```bash
systemctl start serviço
```
#### Parar
```bash
systemctl stop serviço
```

#### Reiniciar
```bash
systemctl restart serviço
```

#### Status
```bash
systemctl status serviço
```


#### Habilitar
```bash
systemctl enable serviço
```
#### Desabilitar
```bash
systemctl disable serviço
```