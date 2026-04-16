+++
title = "Docker & Dockerfile, que dupla hein!"
date = 2022-05-18T07:16:50Z
author = "Allyson Oliveira"
tags = ["docker", "dockerfile", "devops", "linux"]
description = "Bora ver como funciona o docker e o dockerfile?"
+++

Se você está aqui, provavelmente já conhece Docker e suas funcionalidades. Antes de falarmos do dockerfile, deixa eu dar uma pequena introdução dessa ferramenta que está cada vez mais utilizada no dia-a-dia.

**História**

Em um resumo rápido, o docker nasceu por meados de 2013 pela *dotCloud*, que era uma empresa de hospedagem que utilizava containers em todo seu ambiente. Através da tecnologia *LXC* (Linux Containers - já presentes na época), era possível isolar, vários outros "sistemas operacionais", usando chroots, namespaces, jails, etc.Isso deu tanto certo que a empresa resolveu comercializar essa solução. Se você quiser conhecer mais sobre essa história, clica [aqui](https://www.linuxnocafe.com.br/historia-chroot-docker/) 

Você viu alguma semelhança entre docker e máquinas virtuais? Pois é, são conceitos semelhantes e arquiteturas diferentes. Dá uma olhada nessa imagem:

![Arquitetura Docker](/img/arquitetura.png)

Veja a quantidade de camadas que uma arquitetura de virtualização contra a engine de docker? Note que no docker não tem o "Guest OS", que é o sistema operacional virtualizado dentro do VMware. Então, elimine todo o gerenciamento que essa camada precisa (Monitoramento, sustentação, etc). No docker, um sistemas operacional é compartilhado por todos os containers, facilitando o dia-a-dia dos sysadmins/SRE.

Ficou claro? Se não ficou, [#vemdetwitter](https://twitter.com/asoliveira)

**E o que é Dockerfile e onde entra nisso tudo?**

O Dockerfile é um arquivo texto que descreve a engine do docker, os passos necessários para criar uma imagem. Continuando a fazendo um paralelo a virtualização, a imagem do docker é como se fosse uma *golden image do VMware*, onde você pode criar várias máquinas virtuais do mesmo template/imagem.

Já sabe né? Se tiver dúvidas, [#vemdetwitter](https://twitter.com/asoliveira)

Antes de entrar em mais detalhes das instruções do dockerfile, é importante apresentar mais um conceito que é o de camadas.

**O que é camada do dockerfile?**

Algumas instruções do dockerfile gera uma camada adicional na imagem do docker. Uma camada é a instrução (comando). Fazendo um paralelo, imagina que cada pacote instalado no docker é uma camada do docker.

Vamos pegar de exemplo o dockerfile abaixo:

```
FROM UBUNTU
RUN apt-get apache
RUN apt-get install -y emacs
```

Tem essa imagem aqui para facilitar o entendimento:

![DockerFS](/img/dockerfs.png)

O que roda nessas camadas?

Existem duas camadas já prontas no docker, que são:

**1.Bootfs:** É a parte do boot do sistema e do kernel;
**2.rootfs:** Onde fica o arquivo do sistema, como /dev, /proc, /bin

Após esses dois, as duas instruções de RUN (que faz a instalação do apache e do emacs), geram uma camada extra cada um, onde o apache é instalado em uma camada e o emacs em outro. 

Ficou claro esse conceito de camadas? Se tiver dúvidas, [#vemdetwitter](https://twitter.com/asoliveira)

No próximo texto, vou fazer um resumo das instruções do dockerfile! 

Nos vemos por ai!