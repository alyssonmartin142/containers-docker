# Dev Talk - Contêineres Docker 

### Criar um container ubuntu
docker run -it ubuntu

### Criar um container nginx compartilhando a porta 8080
docker run -d --name nginx-demo -p 8080:80 nginx

### Listar os containers em execução
docker ps | docker container ls

### Parar o container
docker stop nginx-demo

### Listar todos os containers
docker ps -a | docker container ls -a

### Iniciar o container
docker start nginx-demo

### Reiniciar o container
docker restart nginx-demo

### Remover o container
docker rm -f nginx-demo

### Ajuda
docker --help
docker start --help | docker COMMAND --help

### Criar um container nginx compartilhando a porta 8080 com volume
### Criar um arquivo de texto ou atulizar na pasta html
docker run -d --name nginx-demo -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html/ nginx
### Alterado o arquivo index.html no vscode e visualizar a alteração no navegador
### Acessar o container
docker exec -it nginx-demo bash
### Rode os comandos no container
```
apt-get update
apt-get install -y vim curl
vim /usr/share/nginx/html/teste.html
curl -I http://localhost:80/teste.html
```

### Remover o container
docker rm -f nginx-demo

### Criar o container novamente para analisarmos que perdemos as configurações realizadas no anterior
docker run -d --name nginx-demo -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html/ nginx

### Remover o container
docker rm -f nginx-demo

### criar dockerfile nginx com instalação do curl e vim, código na pasta exemplo1
```
FROM nginx

RUN apt-get update -y && \
    apt-get install -y vim curl
```

### Realizar o build da imagem
docker build -t <usuario-docker-hub>/nginx-demo:v1 .

### Opcional realizar o push da imagem para o docker hub
docker push <usuario-docker-hub>/nginx-demo:v1

### Rodar o container com a imagem nova, para verificar se está disponível os pacotes
docker run -d --name nginx-demo -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html/ <usuario-docker-hub>/nginx-demo:v1

### criar dockerfile com mais parâmetros, código na pasta exemplo2
```
FROM debian:buster

ENV TESTE ""

RUN apt-get update && \
    apt-get install -y vim \
    curl && \
    apt-get autoremove -y && \
    apt-get auto-clean && \
    rm -rf /var/lib/apt/lists/*

COPY script.sh /etc/entrypoint.sh

RUN ["chmod", "+x", "/etc/entrypoint.sh"]

EXPOSE 80

ENTRYPOINT [ "/bin/bash", "/etc/entrypoint.sh" ]
```

### Realizar o build da imagem
docker build -t <usuario-docker-hub>/nginx-demo:v2 .

### Rodar o container com a imagem nova
docker run -it --name nginx-demo -p 8080:80 -e TESTE="teste2" -v $(pwd)/html:/usr/share/nginx/html/ alyssonmartin142/nginx-demo:v2