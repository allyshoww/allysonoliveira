+++
title = "Embeddings, Vector Database, RAG e MCP: Como os sistemas modernos de IA realmente funcionam"
date = 2026-04-16T10:00:00Z
author = "Allyson Oliveira"
tags = ["IA", "inteligência artificial", "embeddings", "vector database", "RAG", "MCP", "LLM", "AI agents"]
description = "Um resumo do vídeo do ByteMonk explicando como Embeddings, Vector Databases, RAG e MCP se conectam nos sistemas modernos de IA."
+++

Recentemente assisti a um vídeo muito bom do canal [ByteMonk](https://www.youtube.com/@ByteMonk) chamado **"Embeddings, Vector database, Agent, RAG & MCP: How Modern AI Systems Actually Work"** ([link aqui](https://www.youtube.com/watch?v=PByDzuOrkek)) e resolvi trazer um resumo dos conceitos abordados, porque são peças fundamentais para entender como os sistemas de IA modernos funcionam na prática.

## Embeddings: como a IA entende significado

Embeddings são representações numéricas (vetores) que capturam o significado semântico de palavras, frases ou documentos. Em vez de tratar texto como simples sequências de caracteres, os modelos de IA transformam o conteúdo em listas de números onde **conceitos similares ficam próximos no espaço vetorial**.

Na prática, isso significa que uma busca por "aumentar receita" pode encontrar documentos sobre "crescimento de vendas", mesmo sem ter palavras em comum. É isso que permite a busca semântica — ir além do match exato de palavras-chave.

## Vector Databases: onde os vetores moram

Se embeddings são os vetores, as **Vector Databases** são os sistemas de armazenamento otimizados para buscas por similaridade. Diferente de bancos de dados tradicionais que fazem buscas exatas, um vector database encontra os "vizinhos mais próximos" entre milhões de vetores em milissegundos.

Exemplos populares: **Pinecone**, **Weaviate**, **Chroma**, **Qdrant** e **pgvector** (extensão do PostgreSQL).

O caso de uso clássico é armazenar chunks de documentos como vetores pesquisáveis, servindo de base para sistemas de RAG.

## RAG (Retrieval-Augmented Generation): dando contexto ao LLM

RAG é a técnica que conecta tudo isso. O fluxo é elegante:

1. O usuário faz uma pergunta
2. A pergunta é convertida em um embedding
3. O sistema busca documentos relevantes no vector database
4. Esse contexto é injetado no prompt do LLM
5. O modelo gera uma resposta baseada tanto no seu treinamento quanto nas informações recuperadas

O grande benefício do RAG é que o LLM consegue responder com informações **atualizadas e específicas do seu domínio**, sem precisar ser retreinado. Isso resolve um dos maiores problemas dos modelos de linguagem: o conhecimento limitado à data de corte do treinamento.

## MCP (Model Context Protocol): padronizando o acesso a dados

O **MCP** (Model Context Protocol) é um protocolo que padroniza como aplicações de IA acessam fontes de dados externas. Em vez de cada integração precisar de código customizado, o MCP oferece interfaces padronizadas para que agentes de IA se conectem a bancos de dados, APIs e ferramentas de forma segura e consistente.

Enquanto o RAG foca em recuperar informações não-estruturadas para enriquecer prompts, o MCP trata o contexto como **entradas dinâmicas, estruturadas e compostas**, passadas via um protocolo formal. Na prática, RAG e MCP são complementares: o RAG busca o conhecimento relevante e o MCP padroniza como esse conhecimento (e outras ferramentas) chegam até o modelo.

## Como tudo se conecta

A stack moderna de IA funciona assim:

- **Embeddings** transformam dados em vetores com significado semântico
- **Vector Databases** armazenam e buscam esses vetores de forma eficiente
- **RAG** usa essa infraestrutura para dar contexto relevante ao LLM em tempo de consulta
- **MCP** padroniza a comunicação entre o agente de IA e todas essas fontes de dados e ferramentas
- **AI Agents** orquestram tudo isso, planejando consultas em múltiplos passos e adaptando a estratégia conforme os resultados

O vídeo do ByteMonk faz um ótimo trabalho explicando como essas peças se encaixam. Se você está começando a explorar o mundo de IA aplicada, recomendo assistir: [link do vídeo](https://www.youtube.com/watch?v=PByDzuOrkek).

Nos vemos por aí!
