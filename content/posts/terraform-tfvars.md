+++
title = "Tags AWS com o terraform .tfvars"
date = 2020-03-07T07:13:50Z
author = "Allyson Oliveira"
tags = ["terraform", "AWS", "automação", "devops"]
description = "Precisa alterar ou adicionar várias tags de uma vez só? Dá uma espiadinha!"
+++
Esses dias eu tive uma tarefa no trabalho que era adicionar tags em recursos já existentes. Por exemplo:

```
Nome da app: Parasite
Owner: Mr.Kim
```
Na task, falava que todos os recursos da AWS precisam ter essas duas novas tags, para  facilitar o controle financeiro.

Com essa task em wip, pensei que tinham duas maneiras para fazer:

1. Entrar recurso por recurso, inclusive nos ambientes de prod, dev e QA e adicionar essas tags;

2. Editar um arquivo chamado terraform.TFVARS, adicionar as tags necessárias, copiar esse arquivo para todos os recursos e mover a task para done!

![It's magic!](/img/magic.jpeg)

Mas pera lá, o que é o **arquivo .tfvars?**
Eu aprendi que um arquivo com essa extensão é um arquivo de resposta. Eu entendo que uma vez que o terraform passou nos testes, o código está pronto, validado, e esse código que passou nos testes não deve ser mais alterada.

Mas e se você precisar adicionar um novo ip no security group, por exemplo?

Você adiciona esse ip no seu arquivo tfvars, não no variable ou vars.tf. Quer ver a diferença de como fica?

*Arquivo variables.tf sem que seu projeto tenha o arquivo tfvars.*

```
variable "foo" {
default = "bar"
description = "Descrição da minha variável"
}
```    
			
*Arquivo variables.tf com o seu projeto tenha o arquivo tfvars.*

 > variable "tags"{}
 
O mais legal de tudo isso? Assim que deixei pronto um tfvars, fui adicionando o mesmo tfvars em todos os outros recursos. Eu precisei deixar todos os arquivos de variáveis igual ao exemplo acima, mas uma vez pronto, caso a empresa queira add novas tags, tá muito mais fácil adicionar.

Caso você queira deixar apenas as tags no .tfvars, é possível também. Vai ficar como o exemplo acima e usando o arquivo .tfvars abaixo:

> foo = "bar"  

Agora um exemplo de tags:

```
tags = {

  "name"               = "Allyson"
  "email"              = "asoliveira@outlook.com"
}
```
E ai, vocês já usam o .tfvars no código de vocês? Comenta ai! :D