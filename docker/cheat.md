# Docker Cheat Sheet

## Containers

| comando      |  descrição |
|--------------|------------|
| docker ps    |   ver containers ativos |
| docker ps -a |   ver todos containers |
| docker run -d -p port:port imagem | executar um container |
| docker exec -it containerId (sh bash ash) | acessar um container |
| docker logs -f containerId | ver os logs de um container |
| docker inspect containerId | ver detalhess de um container |
| docker stats | ver recursos em uso dos containers | 
| docker stop containerId | parar um container |
| docker start containerId | iniciar um container |
| docker rm containerId | apagar um container | 
| docker rm -f containerId | apagar um container forçadamente |
| docker container prune | limpar containers inativos |

## Imagens

docker build -t user/imagem:v1.0 .
docker build -t user/imagem:v1.0 . -f DockerfileExample

docker image ls

docker image rm imagem
docker image rm -f imagem

docker image prune

## Repositório

docker login

docker push user/imagem:v1.0

docker tag user/imagem:v1.0 user/imagem:latest

docker push user/imagem:latest