+++
title = "Migrando para uma infraestrutura agil"
date = 2022-04-09T07:16:50Z
author = "Allyson Oliveira"
tags = ["AWS", "devops", "SRE", "terraform", "docker", "kubernetes", "migração"]
description = "Nessa série de textos, irei detalhar um pouquinho de como realizar um projeto de migração de app's on-premises para uma infraestrutura agil"
+++

O objetivo desse blog é mostrar algumas tarefas do dia-a-dia como SRE e compartilhar dicas que possam ajudar outras pessoas.

Atualmente, estou em um projeto que visa migrar um ambiente de TI legado e tradicional para uma *infraestrutura agil*. O que isso significa?

O cliente possui uma infraestrutura de  servidores, storage,  switches, hospedado em um datacenter privado da empresa. Esse cenário era ou ainda é muito comum para vários segmentos do mercado.

E como o cliente precisa de agilidade para entrega de novas features e também possui uma dificuldade para mensurar o crescimento de usuários que vão utilizar a app,ele precisa que a *infraestrutura também seja ágil*. Não adianta nada o desenvolvedor entregar uma modificação na app no final da sprint sendo que o time de SRE/Sysadmin's não conseguem acompanhar essa velocidade. Partindo desse principio, escolhemos um provedor de nuvem para suportar essas demandas e modificamos o *"modus operandi"* do time, divulgando a cultura Devops/SRE.

**Como devops não é apenas ferramenta**, realizamos alguns workshops e com ajuda de um agile leader, ministramos algumas dinâmicas para melhorar o trabalho em comunidades  e de maneira colaborativa. Essa é a parte mais dificil, pois muitas pessoas dizem que sabem trabalhar em grupo e que são bons companheiros de trabalhos, mas na verdade, não são. E isso não se resume apenas sobre ajudar o coleguinha junior, mas não é só isso. Eu sei que isso já devia ser educação de cada um, mas ainda tem muita gente racista, xenofóbico e cheio das piadinhas constrangedoras em voz alta. Fora isso, é estar disponível para resolver um problema, entender que quando acontece alguma coisa no seu time você faz parte disso, entre outras coisas!

Temos muito trabalho para fazer, então mãos a obra. Não que esse jeito seja o correto ou o melhor, mas  definimos assim:

Definimos uma stack de ferramentas que utilizariamos. É mais ou menos assim:

![Stack Devops](/img/stack1.png)
![Stack Devops](/img/stack2.png)

Enquanto trabalhavamos em melhorar a cultura da empresa, estavamos realizando o deploy das ferramentas e a primeira que vou começar será o Jenkins.

Quer saber como instalar o Jenkins? Fique ligado nos próximos posts que irei detalhar um pouco mais sobre essa e outras ferramentas.
