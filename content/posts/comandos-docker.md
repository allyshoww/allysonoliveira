+++
title = "Comandos Docker que uso no meu dia-a-dia"
date = 2022-08-18T07:16:50Z
author = "Allyson Oliveira"
tags = ["docker", "devops", "sre", "linux", "kubernetes"]
description = "O que faço com o docker para mover minhas tasks para done?"
+++

Salve, salve! devopeiros? Tudo bem com vocês?  

Vou escrever um pouco sobre os principais comandos que uso no docker, que me ajudam nas tarefas do meu dia-a-dia. Bora lá?

Agora que você já está com o docker instalado, já criou o seu dockerfile, bora administrar seus containers. Abra seu terminal e digita aí!

**docker images**
Lembra o que é imagem? Então, esse comando lista as imagens que você tem na sua máquina/servidor. Algumas informações uteis:

. Tag: é a tag que definimos no momento da criação da imagem.
. Image ID: é um id gerado pelo docker referente a sua imagem.
. Created: Quando essa imagem foi criada.
. Size: É o tamanho da imagem. 

 Se não lembrar, [clica aqui!](https://allysonoliveira.com.br/posts/docker-dockerfile/)

**docker build**
Esse comando builda a imagem a partir do seu dockerfile. Exemplo:
docker build . -t ubuntu:1

Algumas informações:

o '.' significa diretório atual, então, você tem que estar com o terminal aonde você vai buildar a sua imagem. 
o -t é para tag, que é uma label que você quer adicionar na sua imagem. 

E aqui vai uma dica: 
Para facilitar, já construa sua imagem com e as regras do seu repositório, porque quando você for realizar o upload, já está com a tag correta. Veja só o exemplo abaixo:

docker build . -t repositório-do-allyson/imagem-base/app01/python-helloworld:1

**docker push**
Com esse comando, eu envio as imagens docker que eu trabalhei para o registry (Pode ser o registry da AWS ou qualquer outra ferramenta que armazena suas imagens). O comando fica assim:
docker push repositório-do-allyson/imagem-base/app01/python-helloworld:1

**docker pull**
Com o docker pull *nomedaimage* eu baixo a imagem para minha máquina ou servidor para realização do troubleshooting. Fica assim o comando:
docker pull ubuntu

**docker run**
Esse é o comando para você executar um container a partir do seu dockerfile e da sua imagem já construida. Existe uma série de parametros que podem ser uteis para você, mas eu sou basicamente esses aqui:

docker run -it python-helloworld:1

Esse comando você está rodando a imagem que construimos acima, de uma maneira bem simples. Agora, vamos incrementar o comando:

--name: Você define um nome para o container que você está rodando.

-d: Você roda o container com o modo "detachado", não ficando com o terminal preso ao executar o container. 

-p: Você permite conexões externas para dentro do seu container. Deve ser assim:
docker container run -p porta-host:porta-container

-v: quando for necessário montar um volume, deve seguir essa ideia: 
docker container run -v [host/volume/localização]:[container/imagem]

Tem mais algum que você usa? 