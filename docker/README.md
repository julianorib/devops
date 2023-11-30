# Docker Resumido

https://docs.docker.com/engine/

https://www.youtube.com/playlist?list=PLf-O3X2-mxDn1VpyU2q3fuI6YYeIWp5rR


## Comandos

Ver informações sobre containers.
```
docker ps
```
Ver se tem alguma imagem
```
docker images
```
Iniciar um Container Ubuntu em modo Interativo:
```
docker run -i -t ubuntu:22.10 /bin/bash
```
Dentro do Container, executar: 
```
ps -ef
```
Saire finaliza o Container.
```
CTRL +D
```

Sair do Container e o deixa ativo.
```
CTRL+P+Q 
```

Finalizar um Container. (Sem excluir)
```
docker ps 
docker stop IDcontainer
```

Ver o ID do Container
```
docker ps
docker ps -a
```

Iniciar um container existente que foi parado.
```
docker start IDcontainer
```

Executar comandos no Host apontando para o Container.
```
docker exec IDcontainer comando
docker exec IDcontainer ps -ef
docker exec IDcontainer systemctl start nginx
docker exec -it IDcontainer bash
```
Acessar novamente o Container em execução:
```
docker attach IDcontainer
```

Iniciar um Container em Modo Daemon
```
docker container run -d nginx
```

Ver alterações no Container
```
docker diff IDcontainer
```

Iniciar um novo Container e Mapear uma Porta do Container no Host:
```
docker run -d -p 8080:80 ubuntu:22.10  /bin/bash
```

Instalar o NGINX no Container.
```
docker exec -it idContainer bash
```
```
apt update
apt install nginx
/etc/init.d/nginx start
```

Iniciar um novo Container já com NGINX
```
docker run -d -p 8080:80 nginx 
```

Iniciar um Container cuja imagem contem um MySQL, e com variáveis de ambiente.
```
docker run --name mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql
```

Remover definitivamente um Container
```
docker rm IDcontainer
docker rm -f IDcontainer
```

Remover uma Imagem de Container
```
docker images
docker rmi IDimage
docker rmi -f IDimage
```

### Comandos Basicos Troubleshooting

Ver informações detalhadas de um Container
```
docker inspect IDcontainer
```
Ver consumo de CPU, Memoria, Rede, i/o do Container
```
docker stats IDcontainer
```

Ver logs (tudo que passou no Console do Container)
```
docker logs IDcontainer
docker logs -f IDcontainer
docker logs -n 15 IDcontainer
```


### Construindo Imagens

Crie um arquivo Dockerfile dentro de um projeto e construa uma imagem a partir dele.
Exemplo:
```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["src/PedeLogo.Catalogo.Api/PedeLogo.Catalogo.Api.csproj", "src/PedeLogo.Catalogo.Api/"]
RUN dotnet restore "src/PedeLogo.Catalogo.Api/PedeLogo.Catalogo.Api.csproj"
COPY . .
WORKDIR /src/src/PedeLogo.Catalogo.Api
RUN dotnet build "PedeLogo.Catalogo.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "PedeLogo.Catalogo.Api.csproj" -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS final
WORKDIR /app
EXPOSE 80
EXPOSE 443
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "PedeLogo.Catalogo.Api.dll"]
```

```
docker image build -t seuUsuario/projetoX .
```

Ver as imagens depois do commit.
```
docker images ls
```

Iniciar um novo container com a imagem "preparada" .
```
docker run -d -p 8080:80 seuUsuario/projetoX
```

### Docker Registry
hub.docker.com (publico)
<br>
Tem opções para repositorio privado também.

Enviar uma imagem ao Docker Hub.
```
docker login

docker push seuUsuario/projeto1:v1
docker tag seuUsuario/projeto1:v1 seuUsuario/projeto1:latest
docker push seuUsuario/projeto1:latest
```

### Docker Compose

https://docs.docker.com/compose/gettingstarted/

https://docs.docker.com/get-started/08_using_compose/

O docker compose é utilizado para executar um ou mais containers com argumentos necessários como Bind de Portas, Volumes persistentes, Variáveis de Ambiente, entre outros.

Dentro de um projeto, crie um arquivo de compose.yaml.
Exemplo:
```
version: "3.0"

services:
  app:
    image: nginx
    container_name: nginx-app
    restart: always
    depends_on:
      - mysql
   networks:
     - passbolt
    ports:
      - 80:80
    volumes:
      - ./www:/usr/share/nginx/html

  mysql:
    image: mysql
    container_name: mysql-app
    restart: always
    networks:
      - passbolt
    working_dir: ./db
    volumes:
      - ./db:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: teste@123

```
Para executar a aplicação (Containers Aplicação + Banco de Dados):
```
docker compose up -d 
```

### Outros

Para atualizar uma politica de reinicialização:
```
docker update --restart=always IDcontainer
```

Volumes Nomeados (Named volumes)
```
docker volume ls
docker volume create nomevolume
docker run -d -v nomevolume:/var/www/html ubuntu bash
```

Para verificar onde estão Salvos os arquivos do Volume Nomeado.
```
docker volume inspect nomevolume
```

Mapeamento de pastas do S.O. no Container
```
docker run -w /opt/teste -v /opt/teste:/opt/teste -i -t imagemdocontainer
docker run -w /teste -v /opt/teste:/teste -i -t imagemdocontainer
```

Criando redes de Containers.
```
docker network create testenetwork
docker network ls
```