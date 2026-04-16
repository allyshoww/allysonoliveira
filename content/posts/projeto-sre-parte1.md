+++
title = "Migrando para uma infraestrutura agil"
date = 2022-04-09T07:16:50Z
author = "Allyson Oliveira"
tags = ["aws", "devops", "sre", "terraform", "docker", "kubernetes", "migração"]
description = "Nessa série de textos, irei detalhar um pouquinho de como realizar um projeto de migração de app's on-premises para uma infraestrutura agil"
+++

O objetivo desse blog é mostrar algumas tarefas do dia a dia como SRE e compartilhar dicas que possam ajudar outras pessoas.

Atualmente, estou em um projeto que visa migrar um ambiente de TI legado e tradicional para uma *infraestrutura ágil*. O que isso significa?

O cliente possui uma infraestrutura de servidores, storage e switches, hospedada em um datacenter privado da empresa. Esse cenário era (ou ainda é) muito comum para vários segmentos do mercado.

E como o cliente precisa de agilidade para entrega de novas features e também possui dificuldade para mensurar o crescimento de usuários que vão utilizar a app, ele precisa que a *infraestrutura também seja ágil*. Não adianta nada o desenvolvedor entregar uma modificação na app no final da sprint sendo que o time de SRE/Sysadmins não consegue acompanhar essa velocidade. Partindo desse princípio, escolhemos um provedor de nuvem para suportar essas demandas e modificamos o *"modus operandi"* do time, divulgando a cultura DevOps/SRE.

## Cultura primeiro, ferramentas depois

**Como DevOps não é apenas ferramenta**, realizamos alguns workshops e, com ajuda de um agile leader, ministramos algumas dinâmicas para melhorar o trabalho em comunidade e de maneira colaborativa. Essa é a parte mais difícil, pois muitas pessoas dizem que sabem trabalhar em grupo e que são bons companheiros de trabalho, mas na verdade, não são. E isso não se resume apenas a ajudar o coleguinha júnior. Eu sei que isso já deveria ser educação de cada um, mas ainda tem muita gente racista, xenofóbica e cheia de piadinhas constrangedoras em voz alta. Fora isso, é estar disponível para resolver um problema, entender que quando acontece alguma coisa no seu time você faz parte disso, entre outras coisas!

## Definindo a stack

Temos muito trabalho para fazer, então mãos à obra! Não que esse jeito seja o correto ou o melhor, mas definimos assim:

Escolhemos uma stack de ferramentas que utilizaríamos. É mais ou menos assim:

![Stack Devops](/img/stack1.png)
![Stack Devops](/img/stack2.png)

Enquanto trabalhávamos em melhorar a cultura da empresa, estávamos realizando o deploy das ferramentas — e a primeira que vou detalhar será o **Jenkins**.

Quer saber como instalar o Jenkins? Fique ligado nos próximos posts que irei detalhar um pouco mais sobre essa e outras ferramentas.
