# Criando usuários para Administrar um Namespace

Ajuda:\
<https://www.linuxtips.io/blog/descomplicando-rbac-no-kubernetes>

Tenha ou crie um namespace:
```
kubectl create namespace Example
```

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

kubectl config --kubeconfig bob-config set-context bob --cluster=kubernetes --namespace Example --user=bob

kubectl config --kubeconfig bob-config use-context bob

```


Dar permissão ao usuário.

O manifesto abaixo dá permissão geral para um usuário conseguir administrar seus recursos em um determinado namespace.

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: bob-role
rules:
- apiGroups:
  - '*'
  resources:
  - '*'
  verbs:
  - get
  - list
  - watch
  - create
  - update
  - patch
  - delete
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: bob-role-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: bob-role
subjects:
- apiGroup: rbac.authorization.k8s.io
  kind: User
  name: bob
```

Aplique o mesmo:
```
kubectl apply -f user-namespace -n Example
```


# Criando cotas de Recursos para um namespace

Crie um manifesto com o conteúdo abaixo.
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "1"
    limits.memory: 1Gi
```

Aplique com:
```
kubectl apply -f manifest.yaml -n Example
```

