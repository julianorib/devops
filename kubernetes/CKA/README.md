# Exemplos de Perguntas para um exame CKA

Estudar e entender bem os comandos.


## Criar um namespace com a label CKA=true
kubectl create ns teste 
kubectl label ns teste CKA=true

## Criar um deployment "testedeploy" com 2 containers (Nginx e Redis)
kubectl create deployment testedeploy --image nginx --dry-run=client -o yaml > teste.yaml

editar o teste.yaml.
copiar o conteúdo do container nginx 
modificar o conteúdo copiado para redis.
aplicar o deploy teste.yaml


## Testar a conexão entre os Containers
kubectl exec -it teste-xpto -c nginx -n namespace -- bash
curl -v telnet://0:port


## Criar um serviço NodePort na porta 9090
kubectl expose deployment -n teste testedeploy --type NodePort --port 9090 --target-port 80

## Criar uma service account
kubectl create serviceaccount sa1 -n teste


## Alterar a service account do deploy 2
kubectl create serviceaccount sa1 -n teste


## Criar um RBAC permitindo listar os PODs do Namespace 1. 
kubectl create serviceaccount sa1 -n teste
kubectl create role -n teste role1 --resource pod --verb list
kubectl create rolebinding -n teste roleb1 --role role1 --serviceaccount teste:sa1

kubectl run teste --image=curlimages/curl --restart=Never --rm -it -- bash

env
curl -k https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT}/api/version

cd /var/run/secrets/kubernetes.io/serviceaccount
cat token
TOKEN=$(cat token)

curl -k https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT}/api/v1/namespaces/TESTE/pods/ --header "Authorization: Bearer $TOKEN"

