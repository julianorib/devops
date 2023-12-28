## Criando usuários

Criar uma chave
```
openssl genrsa -out bob.key 2048
```

Criar uma Requisição
```
openssl req -new -key bob.key -out bob.csr -subj '/CN=bob'
```

Transformar a requisição em base64
```
cat bob.csr | base64 | tr -d "\n"
```

Criar e aplicar um manifesto com um Objeto "CertificateSigningRequest"
```
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: bob
spec: 
  request: CSR_convertido_base64
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 31536000 #1ano
  usages:
  - client auth
```

Aprovar a Requisição no Kubernetes
```
kubectl certificate approve bob
```

Visualizar a Requisição:
```
kubectl get csr/bob -o yaml
```

Nesta requisição aprovada, será possível visualizar o Certificado gerado pela CA do Kubernetes.

Exportar o Certificado:
```
kubectl get csr/bob -o jsonpath='{.status.certificate}' | base64 -d > bob.crt
```


Exportar configurações para um arquivo de configuração.\
Crie uma cópia do config atual.
```
cp config bob-config
```

Apague o conteudo relacionado ao usuário.\
Começa a partir de "contexts".

Execute os comandos abaixo
```
kubectl config --kubeconfig bob-config set-credentials bob --client-key=bob.key --client-certificate=bob.crt --embed-certs=true

kubectl config --kubeconfig bob-config set-context bob --cluster=kubernetes --user=bob

kubectl config --kubeconfig bob-config use-context bob

```


Dar permissão ao usuário:
```
kubectl create clusterrolebinding bob --clusterrole=cluster-admin --user=bob
kubectl create clusterrolebinding bob --clusterrole=view --user=bob
```


Tipos:
    - cluster-admin
    - admin
    - edit
    - view