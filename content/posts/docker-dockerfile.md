+++
title = "Docker & Dockerfile, que dupla hein!"
date = 2022-05-18T07:16:50Z
author = "Allyson Oliveira"
tags = ["docker", "dockerfile", "devops", "linux"]
description = "Bora ver como funciona o docker e o dockerfile?"
+++

Se você está aqui, provavelmente já conhece Docker e suas funcionalidades. Antes de falarmos do Dockerfile, deixa eu dar uma pequena introdução dessa ferramenta que está cada vez mais utilizada no dia a dia.

## História

Em um resumo rápido, o Docker nasceu por meados de 2013 pela *dotCloud*, que era uma empresa de hospedagem que utilizava containers em todo seu ambiente. Através da tecnologia *LXC* (Linux Containers — já presentes na época), era possível isolar vários outros "sistemas operacionais", usando chroots, namespaces, jails, etc. Isso deu tão certo que a empresa resolveu comercializar essa solução. Se você quiser conhecer mais sobre essa história, clica [aqui](https://www.linuxnocafe.com.br/historia-chroot-docker/).

Você viu alguma semelhança entre Docker e máquinas virtuais? Pois é, são conceitos semelhantes, mas com arquiteturas diferentes. Dá uma olhada nessa imagem:

![Arquitetura Docker](/img/arquitetura.png)

Veja a quantidade de camadas que uma arquitetura de virtualização tem comparada à engine do Docker. Note que no Docker não tem o "Guest OS", que é o sistema operacional virtualizado dentro do VMware. Então, elimine todo o gerenciamento que essa camada precisa (monitoramento, sustentação, etc.). No Docker, um sistema operacional é compartilhado por todos os containers, facilitando o dia a dia dos sysadmins/SREs.

Ficou claro? Se não ficou, [#vemdetwitter](https://twitter.com/asoliveira) 😄

## E o que é Dockerfile e onde entra nisso tudo?

O Dockerfile é um arquivo de texto que descreve, para a engine do Docker, os passos necessários para criar uma imagem. Fazendo um paralelo com a virtualização, a imagem do Docker é como se fosse uma *golden image do VMware*, onde você pode criar várias máquinas virtuais a partir do mesmo template/imagem.

Já sabe né? Se tiver dúvidas, [#vemdetwitter](https://twitter.com/asoliveira)

Antes de entrar em mais detalhes das instruções do Dockerfile, é importante apresentar mais um conceito: o de **camadas**.

## O que é camada do Dockerfile?

Algumas instruções do Dockerfile geram uma camada adicional na imagem do Docker. Uma camada é o resultado de uma instrução (comando). Fazendo um paralelo, imagine que cada pacote instalado no Docker é uma camada.

Vamos pegar de exemplo o Dockerfile abaixo:

```dockerfile
FROM ubuntu
RUN apt-get install apache2
RUN apt-get install -y emacs
```

Tem essa imagem aqui para facilitar o entendimento:

![DockerFS](/img/dockerfs.png)

### O que roda nessas camadas?

Existem duas camadas já prontas no Docker, que são:

1. **bootfs:** É a parte do boot do sistema e do kernel.
2. **rootfs:** Onde fica o sistema de arquivos, como `/dev`, `/proc`, `/bin`.

Após essas duas, as duas instruções de `RUN` (que fazem a instalação do Apache e do Emacs) geram uma camada extra cada uma — o Apache é instalado em uma camada e o Emacs em outra.

Ficou claro esse conceito de camadas? Se tiver dúvidas, [#vemdetwitter](https://twitter.com/asoliveira)

No próximo texto, vou fazer um resumo das instruções do Dockerfile!

Nos vemos por aí!
