# Trivy

Scan de Vulnerabilidades em Containers.

É possivel executar o container apontando para verificar uma imagem.

Exibe um relatório que pode ser exportado em arquivo.

```
docker run aquasec/trivy image mariadb
docker run aquasec/trivy image mariadb > relatorio.txt
```
