+++
title = "Vantagens de utlização de terraform e Infraestrutura como código"
date = 2022-04-24T07:16:50Z
author = "Allyson Oliveira"
tags = ["terraform", "AWS", "infraestrutura como código", "devops", "SRE"]
description = "Nesse texto, vamos entender um pouco sobre mais Terraform e infraestrutura como código."
+++

Conforme falamos no ultimo post sobre nosso projeto(clique [aqui](https://allysonoliveira/posts/projeto-sre-parte1/) para saber mais), hoje vamos fazer a instalação de uma ferramenta que vamos utilizar nesse projeto.

Vamos começar com a instalação do **Terraform**, que é a ferramenta que vamos utilizar para providenciar a *infraestrutura como código*. Mas antes, o que é infraestrutura como código e quais suas vantagens?

Vamos para um exemplo prático:

Para você criar uma máquina na AWS, por exemplo, você precisa de uns 5 minutos no máximo e 10 cliques. Bem simples, né? Agora imagina que você tenha que criar 10, 15, 20 máquinas com as mesmas configurações? Trabalho super repetitivo e de *estagiário* né, rs? Brincadeiras a parte, ninguém merece fazer trabalhos assim!

![Tudo culpa do estagiario](/img/estagiario.png)

Agora imagina que você pode criar uma máquina (ou as 20 máquinas) na AWS com um comando: *terraform plan & terraform apply* ou rodando uma pipeline que faça esse deploy. Top demais né?

Através do *Terraform* e do conceito de infraestrutura como código(IaC), que é utilizar abstrações, uma linguagem poderosa (HCL), reutilizável, confiabilidade, segurança e com baixa probabilidade de erro?

Além disso, imagina o trabalho que é providenciar um hardware para um novo serviço ou ambiente para uma aplicação ou serviço. Instalar sistema operacional, atualizar, instalar libraries, a própria app, etc?

Falando em vantagens, imagina que você tem vários *Security Groups* no seu ambiente. Quando se cria uma nova máquina na AWS, você tem que inserir um *Security Group* ou criar um novo. PAra inserir um novo existente, você tem que digitar o *id* que ele possui ou pesquisar pelo nome. Se você digitar o nome ou o código errado, você pode adicionar um *SG* errado. Com IaC, você escolhe qual recurso você quer usar e seta esse recurso em todas as máquinas com duas linhas, por exemplo:

```HCL
count = "20" # Criação 20 máquinas virtuais iguais
security_groups = ["sg-a1b2c3d4"]
```

Para fazer a instalação do terraform, segue [link] (<https://www.terraform.io/downloads.html)>

Ficou com alguma dúvida ou quer falar sobre esse arquivo? Fala comigo!