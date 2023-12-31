# HELM


## Repositórios

```
helm repo add stable https://charts.helm.sh/stable

helm repo update

helm repo list

helm repo remove nomerepo
```


## Instalando e atualizando

```
helm search repo chart

helm inspect all chart

helm inspect values repo/chart

helm inspect values repo/chart > values.yaml

Normalmente utiliza-se o mesmo nomalues.yaml e do chart com o chartnovo.
helm install chartnovo ralues.yaml epo/chart 

helm install chartnovo repo/chart --values values.yaml

helm install chartnovo repo/chart --set parametro.subparametro=X

helm install chartnovo repo/chart --create-namespace -n nomenamespace

helm update --install chartnovo repo/chart 

helm update --install chartnovo repo/chart --create-namespace -n nomenamespace

helm list ## Mostra os charts instalados com o Helm

helm uninstall chart

helm history chart

helm rollback chart

helm rollback chart 3 ## numero da revision
```

## Criando um Helm Chart

Acesse a pasta do seu projeto
```
helm create nomedochart #template
```

Precisa mover os manifestos para a pasta Template.

Depois precisa modificar alguns itens que poderão ser modificados nos manifestos.

{{ .Release.Name }}
{{ .Release.Name }}-app-deploy

Alguns itens de exemplo:
```
metadata:
  name: {{ .Release.Name }}-app-deploy
spec:
  selector:
    matchLabels:
      app: {{ .Release.Name }}-app
  templates:
    metadata:
      labels:
        app: {{ .Release.Name }}-app
    spec:
      containers:
      - name: nginx
        image: neginx:{{ .Values.nginx.tag }}
        ports:
        - containerPort: 80
        env:
          - name: USER
            values: {{ .Values.nginx.credentials.username }}
          - name: PASS
            values: {{ .Values.nginx.credentials.passwd }}
```

Especificando esse values no arquivo values.yaml:
```
nginx:
  tag: 1.2.3
  credentials:
    username: usuario
    passwd: senha123
```

```
helm install meuapp . --dry-run --debug
helm install meuapp . --dry-run --debug meuapp.yaml
helm install meuapp . --set nginx.credentials.username=fulano --dry-run --debug meuapp.yaml

helm install meuapp .
helm install meuapp . --wait
```

Criando um Secret buscando do Values e deixando em base64 e entre aspas.
```
{{ .Values.app.credentials.username | b64enc | quote }}
{{ .Values.app.credentials.username | b64enc | quote }}
```

### If Else
```
{{- if empty .Values.example }}
faça essa parte do manifesto
{{- else }}
faça essa parte do manifesto
{{- end }}
```

### Range (Loop)
```
{{- range .Values.example.ingress }}
- host: {{ . }}
  service: {{ $.Release.Name }}-service  ## se não colocar o $, ele busca o item do array.
{{- end}}
```

Declarando no Values:
```
example:
  ingress:
  - site1.com
  - site2.com
```

## Dependências de Chart

É possível utilizar um outro Chart como dependência.
Exemplo: Mongodb.

Para isso, deve-se especificar a depenência no arquivo chart.yaml.
```
dependencies:
- name: mongodb
  version: "9.2.5"
  repository: "https://charts.bitnami.com/bitnami"
```

Deve-se atentar para os nomes dos values e alterar no seu projeto.

Por fim, deve-se utilizar o comando abaixo para aplicar:
```
helm dependency build
helm upgrade projeto . --dry-run --debug
```
