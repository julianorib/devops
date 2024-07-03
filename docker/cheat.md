# Docker Cheat Sheet

## Containers

| comando      |  descrição |
|--------------|------------|
| docker ps    |   ver containers ativos |
| docker ps -a |   ver todos containers |

docker run -d -p port:port imagem

docker exec -it containerId (sh bash ash)

docker logs -f containerId

docker inspect containerId

docker stats

docker stop containerId
docker start containerId

docker rm containerId
docker rm -f containerId

docker container prune

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