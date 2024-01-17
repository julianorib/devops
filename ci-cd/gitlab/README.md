# Gitlab

Uma aula sobre CI-CD e Gitlab sensacional:
https://www.youtube.com/watch?v=hnc3GD_gWEA


Para criar uma pipeline CI/CD no Gitlab, você precisa ter uma conta e um projeto. Em seguida, deve-se criar um arquivo no raiz do seu projeto, chamado:

```
.gitlab-ci.yml
```

Para execução da Pipeline, é utilizado os "Runners".\
Há Runners compartilhados, mas o melhor é criar/registrar um Runner.

https://docs.gitlab.com/runner/install/

Após instalar um Runner, é necessário registrar.
```
gitlab-runner register --url endereçoGitLab --token <token>
```

A estrutura de um pipeline no Gitlab é bem simples.

.gitlab-ci.yml
```
stages:
  - build
  - test

job_build:
  stage: build
  script:
    - echo "Building the project"

job_test:
  stage: test
  script:
    - echo "Running tests"
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