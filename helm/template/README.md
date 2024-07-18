# Template do Helm 

Este repositório tem como objetivo ser um template para deploy de aplicações em Kubernetes.

## Como usar:

### Adicionando o repositório:
Crie um token no projeto em: Settings / Access Tokens
```
helm repo add template --username USERNAME --password <access_token>  https://GITLAB.DOMAIN.LOCAL/api/v4/projects/ID/packages/helm/stable
helm repo update
```

### Exportando o values de modelo:
```
helm show values template/template > values.yaml 
```
**Modifique o arquivo values com os dados da aplicação.**

### Testando:
Será gerado os manifestos em tela, para conferência:
```
helm upgrade --install nomeaplicacao template/template --values=values.yaml --dry-run --debug -n nomedoNamespace
```

### Instalando (Deploy):
```
helm upgrade --install nomeaplicacao template/template --values=values.yaml -n nomedoNamespace
```


## Próximos passos

- Criar Probes
- Labels

## Atualizações

- Altere o template/templates/*.yaml conforme necessário.
- Faça os testes locais 
```
cd template/
helm install teste . --dry-run --debug
```

- Faça o commit e push
- Crie a tag
```
git tag v1.x.x
git push --tags
```

Há um CI-CD para fazer o build do pacote helm e upload para este repositório com base na GIT_TAG.
