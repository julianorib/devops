## Registry Privado


Se quiser utilizar um Registry Privado, que não seja do Docker HUB,\
Deve-se cadastrar um Secret com as informações de Login.

```
kubectl create secret docker-registry nomecontasecret --docker-server=server --docker-username=username --docker-password=xyazka --docker-email=email@inteiro.com
```

```
kubectl get secrets
```


Referenciar no final do Bloco de Specs do Manifesto que contém a imagem:

```
imagePullSecrets: 
  - name: nomecontasecret
```
