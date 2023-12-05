# Traefik

O Traefik é um Proxy (Edge Router) para ser utilizado como Ingress Controller.

https://doc.traefik.io/traefik/getting-started/install-traefik/


Para Instalar é bem simples:

```
helm repo add traefik https://traefik.github.io/charts
```

Se quiser visualizar os Values para modificar alguma coisa.
```
helm show values traefik/traefik > traefik.yaml
```
```
helm upgrade --install traefik traefik/traefik --create-namespace -n traefik
```

Será criado um service "traefik" do tipo "loadbalance". \
Visualize este serviço:
```
kubectl get service
```

Registre o IP deste serviço em um DNS público ou privado.


## Certificado SSL Traefik

É possível utilizar um SSL para este DNS (domínio) registrado.\
Se for privado (active directory), gere um certificado assinado com a CA do AD.

Será necessário os arquivos:
- certificado.crt
- chave.key

Crie uma secret com estes arquivos:
```
kubectl create secret generic dominio-secret --from-file=tls.crt=cert-dominio.crt --from-file=tls.key=cert-dominio.key -n traefik
```

Crie um ingressroute com o entryPoint "websecure" e o parametro abaixo na chave "spec":
```
  tls:
    secretName: dominio-secret
```
