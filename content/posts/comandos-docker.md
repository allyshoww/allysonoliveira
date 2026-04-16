+++
title = "Comandos Docker que uso no meu dia-a-dia"
date = 2022-08-18T07:16:50Z
author = "Allyson Oliveira"
tags = ["docker", "devops", "sre", "linux", "kubernetes"]
description = "O que faço com o docker para mover minhas tasks para done?"
+++

Salve, salve devopeiros! Tudo bem com vocês?

Vou escrever um pouco sobre os principais comandos que uso no Docker, que me ajudam nas tarefas do meu dia a dia. Bora lá?

Agora que você já está com o Docker instalado e já criou o seu Dockerfile, bora administrar seus containers. Abre o terminal e digita aí!

---

### docker images

Lembra o que é imagem? Então, esse comando lista as imagens que você tem na sua máquina/servidor. Algumas informações úteis:

- **Tag:** é a tag que definimos no momento da criação da imagem.
- **Image ID:** é um ID gerado pelo Docker referente à sua imagem.
- **Created:** quando essa imagem foi criada.
- **Size:** é o tamanho da imagem.

Se não lembrar, [clica aqui!](https://allysonoliveira.com.br/posts/docker-dockerfile/)

### docker build

Esse comando builda a imagem a partir do seu Dockerfile. Exemplo:

```bash
docker build . -t ubuntu:1
```

Algumas informações:

- O `.` significa diretório atual, então você precisa estar com o terminal no diretório onde vai buildar a sua imagem.
- O `-t` é para tag, que é uma label que você quer adicionar na sua imagem.

**Dica:** para facilitar, já construa sua imagem com as regras do seu repositório, porque quando você for realizar o upload, já vai estar com a tag correta. Veja o exemplo abaixo:

```bash
docker build . -t repositório-do-allyson/imagem-base/app01/python-helloworld:1
```

### docker push

Com esse comando, eu envio as imagens Docker que eu trabalhei para o registry (pode ser o registry da AWS ou qualquer outra ferramenta que armazena suas imagens). O comando fica assim:

```bash
docker push repositório-do-allyson/imagem-base/app01/python-helloworld:1
```

### docker pull

Com o `docker pull` eu baixo a imagem para minha máquina ou servidor para realização de troubleshooting. Fica assim:

```bash
docker pull ubuntu
```

### docker run

Esse é o comando para você executar um container a partir do seu Dockerfile e da sua imagem já construída. Existem vários parâmetros que podem ser úteis, mas eu uso basicamente esses aqui:

```bash
docker run -it python-helloworld:1
```

Esse comando roda a imagem que construímos acima, de uma maneira bem simples. Agora, vamos incrementar:

- **`--name`**: define um nome para o container que você está rodando.
- **`-d`**: roda o container em modo "detached", sem ficar com o terminal preso ao executar o container.
- **`-p`**: permite conexões externas para dentro do seu container. Deve ser assim:

```bash
docker container run -p porta-host:porta-container
```

- **`-v`**: quando for necessário montar um volume, siga essa ideia:

```bash
docker container run -v [host/volume/localização]:[container/imagem]
```

---

Tem mais algum que você usa? Comenta aí!
