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
Se for público, ok.\
Se for privado (active directory), gere um certificado assinado com a CA do AD.

Será necessário os arquivos:
- certificado.crt
- chave.key

Deverá analisar a estratégia de DNS.\
- Se for utilizar servico.dominio.com, e o certificado for wildcard, poderá criar um secret apenas no namespace default e apontar a "secret" no "ingressroute websecure" de cada aplicação.
- Se for utilizar dominio.com/ path, poderá criar um secret apenas no namespace default e apontar a "secret" no "ingressroute websecure" de cada aplicação.

Crie uma secret com os arquivos de certificado:
```
kubectl create secret tls unicoob-secret --cert=certificado.crt --key=certificado-key.pem
```

Em cada aplicação, Crie um ingressroute com o entryPoint "websecure" e o parametro abaixo na chave "spec":
```
  tls:
    secretName: dominio-secret
```
