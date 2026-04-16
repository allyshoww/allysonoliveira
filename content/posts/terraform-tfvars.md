+++
title = "Tags AWS com o terraform .tfvars"
date = 2020-03-07T07:13:50Z
author = "Allyson Oliveira"
tags = ["terraform", "aws", "automação", "devops"]
description = "Precisa alterar ou adicionar várias tags de uma vez só? Dá uma espiadinha!"
+++

Esses dias eu tive uma tarefa no trabalho que era adicionar tags em recursos já existentes. Por exemplo:

```
Nome da app: Parasite
Owner: Mr.Kim
```

Na task, falava que todos os recursos da AWS precisavam ter essas duas novas tags, para facilitar o controle financeiro.

Com essa task em WIP, pensei que tinham duas maneiras de fazer:

1. Entrar recurso por recurso, inclusive nos ambientes de prod, dev e QA, e adicionar essas tags.
2. Editar um arquivo chamado `terraform.tfvars`, adicionar as tags necessárias, copiar esse arquivo para todos os recursos e mover a task para done!

![It's magic!](/img/magic.jpeg)

## Mas pera lá, o que é o arquivo .tfvars?

Eu aprendi que um arquivo com essa extensão é um **arquivo de resposta**. Eu entendo que, uma vez que o Terraform passou nos testes, o código está pronto, validado, e esse código que passou nos testes não deve ser mais alterado.

Mas e se você precisar adicionar um novo IP no security group, por exemplo?

Você adiciona esse IP no seu arquivo `.tfvars`, **não** no `variables.tf` ou `vars.tf`. Quer ver a diferença de como fica?

**Arquivo `variables.tf` sem que seu projeto tenha o arquivo `.tfvars`:**

```hcl
variable "foo" {
  default     = "bar"
  description = "Descrição da minha variável"
}
```

**Arquivo `variables.tf` quando seu projeto tem o arquivo `.tfvars`:**

```hcl
variable "tags" {}
```

O mais legal de tudo isso? Assim que deixei pronto um `.tfvars`, fui adicionando o mesmo arquivo em todos os outros recursos. Eu precisei deixar todos os arquivos de variáveis igual ao exemplo acima, mas uma vez pronto, caso a empresa queira adicionar novas tags, está muito mais fácil!

Caso você queira deixar apenas as tags no `.tfvars`, é possível também. O `variables.tf` fica como o exemplo acima e o arquivo `.tfvars` fica assim:

```hcl
foo = "bar"
```

Agora um exemplo com tags:

```hcl
tags = {
  "name"  = "Allyson"
  "email" = "asoliveira@outlook.com"
}
```

E aí, vocês já usam o `.tfvars` no código de vocês? Comenta aí! :D
