# Exemplos de Perguntas para um exame CKA

## Criar um namespace com a label CKA=true
```
kubectl create ns teste 
kubectl label ns teste CKA=true
```

## Criar um deployment "testedeploy" com 2 containers (Nginx e Redis)
```
kubectl create deployment testedeploy --image nginx --dry-run=client -o yaml > teste.yaml
```
Editar o teste.yaml.
Copiar o conteúdo do container nginx 
Modificar o conteúdo copiado para redis.
Aplicar o deploy teste.yaml

## Testar a conexão entre os Containers
```
kubectl exec -it testedeploy -c nginx -n teste -- bash
```
```
curl -v telnet://0:port
```

## Criar um serviço NodePort na porta 9090
```
kubectl expose deployment testedeploy --type NodePort --port 9090 --target-port 80 -n teste
```

## Criar uma service account
```
kubectl create serviceaccount sa1 -n teste
```

## Alterar a service account do deploy 2
```
kubectl set serviceaccount deploy testedeploy sa1 -n teste
```

## Deploy, Service, SA, Role, Rolebinding, Token
```
kubectl create deploy nginx --image nginx -n temporario
kubectl expose deploy nginx --port=80 --target-port=80 --type=LoadBalancer -n temporario

kubectl create sa nginxsa -n <namespace> -n temporario
kubectl set sa deploy nginx nginxsa -n temporario

kubectl create role nginxRole --resource pod --verb list -n temporario
kubectl create rolebinding nginxRoleBinding --role nginxRole --serviceaccount temporario:nginx -n temporario

kubectl create token juliano

TOKEN="<token-gerado>"

curl -k https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT}/api/v1/namespaces/temp/pods/ --header "Authorization: Bearer $TOKEN"
```