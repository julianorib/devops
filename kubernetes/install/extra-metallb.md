# LoadBalancer para o Kind (Metallb)

O kind permite que se crie um LoadBalancer para fins de testes e estudos.

https://kind.sigs.k8s.io/docs/user/loadbalancer/


## Configuração

Aplique este manifesto abaixo:
```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.7/config/manifests/metallb-native.yaml
```

Verifique o Pool de Endereço que poderá ser utilizado:
```
docker network inspect -f '{{.IPAM.Config}}' kind
```
A saida deve ser algo como:

172.18.0.0/16 ou 
<br>
172.19.0.0/16

Crie um arquivo chamado metallb.yaml com o conteúdo abaixo:
```
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: example
  namespace: metallb-system
spec:
  addresses:
  - 172.19.255.200-172.19.255.250
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: empty
  namespace: metallb-system
```
*altere a linha do ip address, informando a rede de acordo com o valor obtido no passo anterior.*

Aplique o manifesto:
```
kubectl apply -f metallb.yaml
```

Para testar o LoadBalancer, execute o manifesto abaixo:
*Este manifesto tem porta padrão 5678*
```
kubectl apply -f https://kind.sigs.k8s.io/examples/loadbalancer/usage.yaml
```

Veja o IP que o LoadBalancer obteve para o Serviço:
```
kubectl get service
```

```
NAME          TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
foo-service   LoadBalancer   10.96.134.133   172.18.255.200   5678:32386/TCP   8s
kubernetes    ClusterIP      10.96.0.1       <none>           443/TCP          31m
```

Verifique o IP na coluna External-IP.

Abra o Navegador e informe o IP + porta do Manifesto:

http://172.18.255.200:5678

*Se aparecer alguma coisa é porque funcionou.*

