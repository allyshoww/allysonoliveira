+++
title = "Vantagens de utlização de terraform e Infraestrutura como código"
date = 2022-04-24T07:16:50Z
author = "Allyson Oliveira"
tags = ["terraform", "AWS", "infraestrutura como código", "devops", "SRE"]
description = "Nesse texto, vamos entender um pouco sobre mais Terraform e infraestrutura como código."
+++

Conforme falamos no último post sobre nosso projeto (clique [aqui](https://allysonoliveira.com.br/posts/projeto-sre-parte1/) para saber mais), hoje vamos falar sobre uma ferramenta que vamos utilizar nesse projeto.

Vamos começar com o **Terraform**, que é a ferramenta que vamos utilizar para provisionar a *infraestrutura como código*. Mas antes, o que é infraestrutura como código e quais são suas vantagens?

## Um exemplo prático

Para você criar uma máquina na AWS, por exemplo, você precisa de uns 5 minutos no máximo e 10 cliques. Bem simples, né? Agora imagina que você tenha que criar 10, 15, 20 máquinas com as mesmas configurações? Trabalho super repetitivo e de *estagiário*, né? rs. Brincadeiras à parte, ninguém merece fazer trabalhos assim!

![Tudo culpa do estagiário](/img/estagiario.png)

Agora imagina que você pode criar uma máquina (ou as 20 máquinas) na AWS com um comando: `terraform plan && terraform apply` ou rodando uma pipeline que faça esse deploy. Top demais, né?

Através do *Terraform* e do conceito de **Infraestrutura como Código (IaC)**, é possível utilizar abstrações, uma linguagem poderosa (HCL), reutilizável, com confiabilidade, segurança e baixa probabilidade de erro.

Além disso, imagina o trabalho que é provisionar um hardware para um novo serviço ou ambiente para uma aplicação. Instalar sistema operacional, atualizar, instalar libraries, a própria app, etc.

## Vantagens na prática

Imagina que você tem vários *Security Groups* no seu ambiente. Quando se cria uma nova máquina na AWS, você tem que inserir um *Security Group* ou criar um novo. Para inserir um existente, você tem que digitar o *ID* que ele possui ou pesquisar pelo nome. Se você digitar o nome ou o código errado, pode adicionar um *SG* errado. Com IaC, você escolhe qual recurso quer usar e seta esse recurso em todas as máquinas com duas linhas, por exemplo:

```hcl
count           = 20 # Criação de 20 máquinas virtuais iguais
security_groups = ["sg-a1b2c3d4"]
```

Para fazer a instalação do Terraform, segue o [link da documentação oficial](https://www.terraform.io/downloads.html).

Ficou com alguma dúvida ou quer falar sobre esse assunto? Fala comigo!
