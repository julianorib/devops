# Gitlab

Para criar uma pipeline CI/CD no Gitlab, você precisa ter uma conta e um projeto. Em seguida, deve-se criar um arquivo no raiz do seu projeto, chamado:

```
.gitlab-ci.yml
```

A estrutura de um pipeline no Gitlab é bem simples.

.gitlab-ci.yml
```
stages
    - test
    - build
    - deploy

Testes 1:                 # pode haver mais de 1 estágio com vários nomes, porém referenciando o mesmo estágio (test).
    stage: test
    image: caminhodaimagemDocker
    script: ## comandos linux
     - export VARIAVEL=valor
     - export VARIAVEL2=valor2
     - echo "mensagem de teste" || true

Docker Build:
    stage: build
    tags:
      - shell           # Não entendi ainda este item
    before_script:
      - docker login -u $VAR_REPO_USER -p $VAR_REPO_PASS $VAR_REPO
    script:
      - docker build -t $VAR_REPO/nomedaimagem:$CI_PIPELINE_ID .
      - docker build -t $VAR_REPO/nomedaimagem:latest .
      - docker push $VAR_REPO/nomedaimage:$CI_PIPELINE_ID
    only:
      - main            # Referenciando que só irá executar na Branch main

Deploy Kubernetes:
    stage: deploy
    image: bitnami/kubectl:1.14 # Exemplo
    before_script:
      - export KUBECONFIG=$VAR_KUBE_TOKEN
    script:
      - kubectl apply -f . -R 

```

## Variáveis

Há algumas variáveis do próprio Gitlab que podem ser utilizadas no código.\
Para definir as suas variáveis, no menu esquerdo, Settings, CI/CD, Variáveis


## Pipeline Completa 

Um exemplo de estágios para uma pipeline super completa.\
Um dia, chegamos lá.

- Test Smoke
- Test Unitário
- Test Style
- Test Smell
- Test Sonar

- Build

- Image Docker build
- Image Docker check
- Image Docker clean

- Push Docker

- Deploy K8S

- Test Carga

- Test Aceitação (selenium)

- Test Security (cve-scanner)

- Notify Team