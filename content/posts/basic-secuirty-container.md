+++
title = "Começanco com segurança em container"
date = 2023-04-08T07:16:50Z
author = "Allyson Oliveira"
tags = ["docker", "segurança", "devsecops", "kubernetes", "containers"]
description = "Bora começar a deixar os seus containers mais seguros?"
+++


O *Docker* é uma plataforma de software que permite a criação, o teste e a implantação de aplicações rapidamente. Se você quer conhecer mais sobre essa tecnologia, pode clicar nesse [link](https://www.docker.com). 

Sendo uma tecnologia muito utilizada,  devemos ter uma olhar critico com relação a segurança.  Antigamente,  existia uma grande discussão de entrega de velocidade de software versus segurança, onde se tinha uma visão de que para entregar uma *aplicação agil e moderna, temos que abrir mão de segurança*, o que **não é verdade**,  pois hoje, principalmente com ambiente em nuvem, não é necessário deixar a segurança da informação de lado, sendo possível entregar a aplicação de maneira segura em todas as etapas. 

Iremos detalhar  alguns pontos que temos que estar atento:

•	Rode o Docker no modo "Rootless Mode"
Primeiramente, o Rootless mode significa você rodar o daemon do docker e qualquer container com um usuário sem privilégio de root, sendo totalmente importante para mitigar vulnerabilidades. Maiores informações podem ser encontradas aqui.   

Para você rodar o seu Docker in rootless mode:
1.	Instale o Docker em modo "rootless" - veja as instruções clicando aqui;
2.	Use esse comando para dar launch no daemon do docker quando o host iniciar:
systemctl --user enable docker| sudo loginctl enable-linger $(whoami)
3. Para rodar um container com rootless:
docker context use rootless
docker run -d -p 8080:80 nginx

Para saber se seu container está rodando com privilégio root, rode o seguinte comando:
docker inspect --format=’{{.HostConfig.Privileged}}’ $containerID
Se a resposta desse comando for false, a configuração está correta. Caso seja true, o ideal é parar o container e rodar ele como rootless(ou refazer a instalação do docker)

•	Limites de containers
É fundamental rodar os containeres com limite de recursos, pois  caso algum container seja invadido e se tenha acesso ao ambiente, o container não pode usar todo o recurso de CPU e memória disponivel. Essa configuração tem que ser feita na hora do docker run, conforme documentação oficial

•	Isolamento de container
O isolamento do container preveni que um acesso indevido ao host ou a outros containers. Para melhorar ainda mais o isolamento de outros containers, podemos utilizar algumas ferramentas:

o	Linux Namespace:  Com o Linux Namespace, é possível criar um contexto dentro de um sistema operacional isolado de todos outros namespaces/recursos do host fisico. 
o	SELinux: O Selinux permite que os administradores de sistema defina mais controle de acesso a aplicação, processos e arquivos dentro do sistema operacional. 
o	AppArmor:  É um sistema de controle mandatório de acesso, onde o Kernel consulta o App Armor para cada consulta do sistema para saber se o processo está autorizado a fazer ou não. 
o	Cgroups: Os Cgroups permitem alocar recursos, tais como CPU e memória, entre grupos de usuários ou processos rodando em um sistema operacional

•	Configurar Filesystem e Volumes como read-only
Para evitar que algum malware ou ataque malicioso faça alguma modificação no seu container ou no seu file system, rode os volumes como read-only, sempre que o container não precise escrever no volume. Use o seguinte comando para isso: 

docker run --read-only 

Maiores informações nesse link

•	Vulnerabilidade de imagens 
CVE (Commom Vulnerabilities and Exposures) é um banco de dados que registra vulnerabilidades e exposições relacionadas a segurança da informação que são reconhecidas publicamente e checar as vulnerabilidades  é um dos itens obrigatórios para validar se a imagem que está sendo utilizada está vulnerável a ameaças que já foram descobertas.  Caso o seu repositório  seja o  AWS Elastic Container Registry (ECR), você pode habilitar o scan toda vez que tenha um push.  

Com o ECR, você pode criar o seu repositório já com o scan de imagens habilitados ou habiltar em um repositório que já esta criado, tanto via AWS Console ou AWS CLI. Maiores informações podem ser encontradas nesse link. 

Você também pode usar ferramentas open source para essa etapa de segurança da sua pipeline.  Outro item importante é ter o controle de qual usuário pode fazer upload de imagens no seu ECR, seja através de RBAC (Role Based Access Control) ou outro tipo de segurança e permissionamento. 

•	Cuidado com dados sensiveis nas suas imagens docker
Não armazene dados sensiveis em suas imagens, como crendenciais, SSH, certificados. A AWS possui uma série de serviços de segurança que podem te ajudar a gerenciar essas informações na sua aplicação.  

•	Salve os dados dos containers separadamente
Eu sei que é muito tentador se conectar através do SSH para resolver um problema em algum host ou aplicação que está executando algum container.  Mas liberar o SSH, mesmo com regras de firewall/security groups limitando o ip de origem, não é uma boa prática de segurança. Nesse caso, é bem melhor exportar os logs para alguma ferramenta de SIEM ou outro local (Cloudtrail, por exemplo) e fazer o troubleshooting sem conectar nas máquinas.

Conclusão
Com esse texto, já é possível melhorar a segurança dos containers que vocês estão executando. Não perca os próximos textos, pois vamos explorar ainda mais outros conceitos  e deixar o ambiente cada vez mais seguro!

