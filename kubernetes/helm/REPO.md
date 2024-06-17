# Criando um Helm Chart 

Este guia tem como objetivo mostrar como criar um Helm Chart de aplicação e hospedar em um Gitlab registry.


Criação do Chart
```
helm create <examples>
```

Mantenha no local a pasta Templates, .helmignore, Chart.yaml, values.yaml.\
O restante pode apagar.\
Dentro de Templates, apague tudo e coloque os manifestos da aplicação.\
Modifique conforne necessidade. 

Faça o build da aplicação.
```
helm package <example>
```

Crie um repositório no Gitlab e crie um access_token para o usuário. (access_token do projeto não funciona).\
Dê permissão na API.
```
helm repo add <example> --username <username> --password <access_token>  https://gitlab.example.com/api/v4/projects/<project_id>/packages/helm/stable
```

Instale um plugin para conseguir fazer o Push no repositório.
```
helm plugin install https://github.com/chartmuseum/helm-push
```
Faça o push no repositório.
```
helm cm-push <example>-0.1.0.tgz example
```

Para instalar a partir do repostório agora, faça o update do repositório.
```
helm repo update
```

Faça a instalação:
```
helm upgrade --install <test> <example>/<example> --values=values.yaml --dry-run --debug -n exampleNamespace
helm upgrade --install <test> <example>/<example> --values=values.yaml -n exampleNamespace
```

Para deinstalar:
```
helm uninstall <test>
```

## Ref:
<https://docs.gitlab.com/ee/user/packages/helm_repository/>

<https://helm.sh/docs/intro/using_helm/#creating-your-own-charts>

<https://about.gitlab.com/blog/2021/10/18/improve-cd-workflows-helm-chart-registry/>
