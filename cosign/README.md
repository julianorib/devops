# Assinando suas imagens Docker

Para isso será utilizado o Cosign.

https://github.com/sigstore/cosign


## Instalar o Cosgin

binário


## Criar par de chaves

cosign generate-key-pair

cosign.key
cosign.pub

## Fazer o build e Push de uma Imagem

docker build -t julianorib/imagemteste:v1.0 .
docker push julianorib/imagemteste:v1.0

## Assinar

cosign sign -key cosign.key julianorib/imagemteste:v1.0

## Validar assinatura

cosign verify --key cosign.pub julianorib/imagemteste:v1.0


## Kubernetes 

No kubernetes pode-se aceitar somente imagens assinadas.\
Ver como fazer.