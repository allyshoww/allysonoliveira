+++
title = "Começanco com segurança em container"
date = 2023-04-08T07:16:50Z
author = "Allyson Oliveira"
tags = ["docker", "segurança", "devsecops", "kubernetes", "containers"]
description = "Bora começar a deixar os seus containers mais seguros?"
+++

O *Docker* é uma plataforma de software que permite a criação, o teste e a implantação de aplicações rapidamente. Se você quer conhecer mais sobre essa tecnologia, pode clicar nesse [link](https://www.docker.com).

![Segurança em Containers](/img/seguranca-containers.png)

Sendo uma tecnologia muito utilizada, devemos ter um olhar crítico com relação à segurança. Antigamente, existia uma grande discussão sobre velocidade de entrega de software versus segurança, onde se tinha a visão de que para entregar uma *aplicação ágil e moderna, temos que abrir mão de segurança* — o que **não é verdade**. Hoje, principalmente com ambientes em nuvem, não é necessário deixar a segurança da informação de lado, sendo possível entregar a aplicação de maneira segura em todas as etapas.

Vamos detalhar alguns pontos que temos que ficar atentos:

---

## 1. Rode o Docker no modo "Rootless Mode"

O Rootless mode significa rodar o daemon do Docker e qualquer container com um usuário **sem privilégio de root**, sendo totalmente importante para mitigar vulnerabilidades.

Para rodar o seu Docker em rootless mode:

1. Instale o Docker em modo "rootless" — veja as instruções na documentação oficial.
2. Use esse comando para iniciar o daemon do Docker quando o host iniciar:

```bash
systemctl --user enable docker
sudo loginctl enable-linger $(whoami)
```

3. Para rodar um container com rootless:

```bash
docker context use rootless
docker run -d -p 8080:80 nginx
```

Para saber se seu container está rodando com privilégio root, rode o seguinte comando:

```bash
docker inspect --format='{{.HostConfig.Privileged}}' $containerID
```

Se a resposta for `false`, a configuração está correta. Caso seja `true`, o ideal é parar o container e rodá-lo como rootless (ou refazer a instalação do Docker).

## 2. Limites de containers

É fundamental rodar os containers com **limite de recursos**, pois caso algum container seja invadido e se tenha acesso ao ambiente, o container não poderá usar todo o recurso de CPU e memória disponível. Essa configuração deve ser feita na hora do `docker run`, conforme a [documentação oficial](https://docs.docker.com/config/containers/resource_constraints/).

## 3. Isolamento de container

O isolamento do container previne que um acesso indevido chegue ao host ou a outros containers. Para melhorar ainda mais o isolamento, podemos utilizar algumas ferramentas:

- **Linux Namespace:** com o Linux Namespace, é possível criar um contexto dentro de um sistema operacional isolado de todos os outros namespaces/recursos do host físico.
- **SELinux:** permite que os administradores de sistema definam mais controle de acesso a aplicações, processos e arquivos dentro do sistema operacional.
- **AppArmor:** é um sistema de controle mandatório de acesso, onde o kernel consulta o AppArmor para cada chamada do sistema para saber se o processo está autorizado ou não.
- **Cgroups:** permitem alocar recursos, tais como CPU e memória, entre grupos de usuários ou processos rodando em um sistema operacional.

## 4. Configure Filesystem e Volumes como read-only

Para evitar que algum malware ou ataque malicioso faça alguma modificação no seu container ou no seu filesystem, rode os volumes como **read-only** sempre que o container não precise escrever no volume. Use o seguinte comando:

```bash
docker run --read-only <imagem>
```

Maiores informações na [documentação oficial](https://docs.docker.com/engine/reference/commandline/run/).

## 5. Vulnerabilidade de imagens

**CVE** (Common Vulnerabilities and Exposures) é um banco de dados que registra vulnerabilidades e exposições relacionadas à segurança da informação reconhecidas publicamente. Checar as vulnerabilidades é um dos itens **obrigatórios** para validar se a imagem que está sendo utilizada está vulnerável a ameaças já descobertas.

Caso o seu repositório seja o **AWS Elastic Container Registry (ECR)**, você pode habilitar o scan toda vez que houver um push. Com o ECR, você pode criar o seu repositório já com o scan de imagens habilitado ou habilitar em um repositório já existente, tanto via AWS Console quanto via AWS CLI. Maiores informações na [documentação do ECR](https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-scanning.html).

Você também pode usar ferramentas open source para essa etapa de segurança da sua pipeline. Outro item importante é ter o controle de qual usuário pode fazer upload de imagens no seu ECR, seja através de **RBAC** (Role Based Access Control) ou outro tipo de permissionamento.

## 6. Cuidado com dados sensíveis nas suas imagens Docker

Não armazene dados sensíveis em suas imagens, como credenciais, chaves SSH ou certificados. A AWS possui uma série de serviços de segurança que podem te ajudar a gerenciar essas informações na sua aplicação (como o **AWS Secrets Manager** e o **AWS Systems Manager Parameter Store**).

## 7. Salve os dados dos containers separadamente

Eu sei que é muito tentador se conectar via SSH para resolver um problema em algum host ou aplicação que está executando um container. Mas liberar o SSH, mesmo com regras de firewall/security groups limitando o IP de origem, não é uma boa prática de segurança. Nesse caso, é bem melhor exportar os logs para alguma ferramenta de SIEM ou outro local (CloudTrail, por exemplo) e fazer o troubleshooting sem conectar nas máquinas.

---

## Conclusão

Com esse texto, já é possível melhorar a segurança dos containers que vocês estão executando. Não perca os próximos textos, pois vamos explorar ainda mais outros conceitos e deixar o ambiente cada vez mais seguro!
