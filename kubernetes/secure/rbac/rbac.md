# Service Account para PODs

https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/

Uma conta de serviço fornece uma identidade para processos executados em um POD e é mapeada para um objeto ServiceAccount.

Se você não especificar uma ServiceAccount ao criar um POD, o Kubernetes atribuirá automaticamente a ServiceAccount default nesse namespace.


## Comandos
```
kubectl get serviceaccount
kubectl describe pod
```

Procurar a linha Service Account no descibe

# Criando uma role (RBAC)

https://kubernetes.io/docs/reference/access-authn-authz/rbac/

O controle de acesso baseado em funções (RBAC) é um método de regular o acesso a recursos de computador ou rede com base nas funções de usuários individuais em sua organização.

Temos 2 opções de permissionamento.

- A nível de recursos
- A nível de Cluster

Em resumo, é necessário criar os seguintes itens:

- serviceAccount
- role
- roleBinding

## Comandos

```
kubectl get role
kubectl describe role
```
