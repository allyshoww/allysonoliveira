+++
title = "Kiro: da IDE ao terminal — spec-driven development, agents e o ecossistema completo"
date = 2026-04-20T10:00:00Z
author = "Allyson Oliveira"
tags = ["kiro", "inteligência artificial", "developer tools", "CLI", "IDE", "agents", "MCP", "spec-driven development", "produtividade"]
description = "Conheça o ecossistema Kiro: a IDE com spec-driven development e o CLI com agents, steering files, knowledge base, hooks, MCP servers e muito mais."
+++

O **Kiro** é um ecossistema de desenvolvimento assistido por IA criado pela AWS que existe em duas formas: uma **IDE** (fork do VS Code) focada em spec-driven development, e uma **CLI** poderosa que roda direto no terminal. Ambas compartilham conceitos como agents, steering files, hooks e MCP servers, mas cada uma tem seu ponto forte. Neste post vou apresentar as principais features de ambas.

## Kiro IDE: spec-driven development

O Kiro IDE foi lançado em preview em julho de 2025. É um fork do Code OSS (a base open-source do VS Code) e faz uma aposta específica: o maior problema das ferramentas de IA para código não é que elas não conseguem gerar código — é que geram código **sem um plano**.

### O que é Spec-Driven Development (SDD)

Enquanto o "vibe coding" (prompting direto e ver o que sai) funciona bem para protótipos, ele tem problemas sérios em projetos reais: código verboso, inconsistente, sem aderência a padrões de arquitetura e sem testes. O SDD resolve isso adicionando uma **fase de planejamento antes de qualquer código ser escrito**.

O fluxo funciona assim:

1. **Você descreve a feature** em linguagem natural — "Adicionar um sistema de reviews para produtos" já é suficiente
2. **Kiro gera três artefatos** dentro de `.kiro/specs/<feature>/`:
   - `requirements.md` — user stories com critérios de aceitação usando notação EARS
   - `design.md` — documento de design técnico com diagramas e schemas
   - `tasks.md` — lista numerada de tarefas de implementação
3. **Você revisa e edita** os specs antes de qualquer código ser escrito
4. **O agent implementa** tarefa por tarefa, seguindo o plano

O ponto crítico: a revisão do spec acontece **antes** de qualquer código. Corrigir um spec leva 5 minutos. Corrigir três dias de código construído sobre uma premissa errada leva muito mais.

### Autopilot

Por padrão, o agent pede aprovação a cada ação significativa. O modo **Autopilot** remove esses checkpoints — o agent executa a lista de tarefas do spec sem pausar. Útil quando você revisou o spec com cuidado e confia no plano de implementação.

### Modelos e preços

O Kiro IDE usa Claude Sonnet 4.5 como modelo principal e tem um modo **Auto** que roteia para o modelo mais custo-efetivo dependendo da complexidade do prompt. O pricing começa com um tier gratuito (50 interações/mês), Pro a $19/mês (1.000 interações) e Pro+ a $39/mês (3.000 interações).

### Compatibilidade

Por ser baseado no Code OSS, o Kiro IDE aceita configurações do VS Code e plugins compatíveis com Open VSX. Se você já tem um `.vscode/settings.json`, ele funciona direto.

## Kiro CLI: o poder no terminal

O Kiro CLI é a versão terminal do ecossistema. Você inicia uma conversa com `kiro-cli chat` e a partir daí pode criar arquivos, refatorar código, rodar testes, investigar bugs, fazer chamadas à AWS e muito mais — tudo sem sair do terminal.

O diferencial é que ele não é um assistente genérico: você pode configurá-lo profundamente para se adaptar ao seu projeto, stack e fluxo de trabalho.

## Agents: personalidades especializadas

Uma das features mais poderosas do Kiro são os **agents**. Um agent é um arquivo JSON que define o comportamento do assistente: quais ferramentas ele pode usar, qual o system prompt, quais arquivos de contexto ele carrega e quais integrações ele tem.

### Exemplo: agent de segurança

Imagine que você quer um agent especializado em security review. Crie `.kiro/agents/sec-reviewer.json`:

```json
{
  "name": "sec-reviewer",
  "description": "Security reviewer - analisa código buscando vulnerabilidades",
  "prompt": "You are a senior security engineer. Analyze code for vulnerabilities: injection flaws, broken auth, secrets exposure, insecure dependencies, missing input validation, and OWASP Top 10 issues. Be specific about the risk and suggest fixes with code examples. Never modify files directly — only report findings.",
  "tools": ["fs_read", "grep", "glob", "code"],
  "allowedTools": ["fs_read", "grep", "glob", "code"],
  "resources": ["file://src/**/*", "file://package.json", "file://**/requirements.txt"],
  "keyboardShortcut": "ctrl+shift+s",
  "welcomeMessage": "Security reviewer ativo. Qual código você quer que eu analise?"
}
```

Note que esse agent tem **apenas ferramentas de leitura** — ele não pode escrever arquivos nem executar comandos. Isso é intencional: um reviewer não deve modificar código, apenas reportar.

### Workflow com o agent de segurança

1. **Ative o agent** — `/agent swap sec-reviewer` ou `Ctrl+Shift+S`
2. **Peça a análise** — "Analise o módulo de autenticação em src/auth/"
3. **O agent investiga** — ele lê os arquivos, busca padrões perigosos com grep e code intelligence
4. **Receba o relatório** — vulnerabilidades encontradas com severidade, explicação e sugestão de fix
5. **Volte ao agent de dev** — `/agent swap` ou o atalho do seu agent principal para implementar as correções

Você pode ir além e criar um **hook** que roda o sec-reviewer automaticamente antes de cada commit, ou usar **sub-agents** para criar um pipeline: dev implementa → sec-reviewer analisa → dev corrige.

### Agents built-in

Existem também agents que já vêm com o Kiro:
- **kiro_default** — o agent padrão com acesso a todas as ferramentas
- **kiro_guide** — responde perguntas sobre o próprio Kiro usando a documentação embutida
- **kiro_planner** — ajuda a quebrar ideias em planos de implementação (acessível via `/plan`)

## Steering: guiando o comportamento do agente

Os **steering files** são arquivos Markdown que ficam em `.kiro/steering/` e são carregados automaticamente no contexto do agent padrão. Eles servem para dar instruções persistentes ao Kiro sobre como ele deve se comportar no seu projeto — é como um "manual do projeto" que o assistente sempre consulta.

Exemplo de um `.kiro/steering/project.md`:

```markdown
# Stack
- Python 3.12, FastAPI, Pydantic v2
- DynamoDB single-table design (table: AppTable, region: us-east-1)
- Lambda + API Gateway, deployed via AWS SAM
- Structured logging via structlog, JSON output

# Convenções de código
- Todas as funções têm type annotations
- Error responses: {"error": "SNAKE_CASE_CODE", "message": "texto legível"}
- Testes usam pytest com pytest-anyio para async; fixtures em conftest.py
- Sem print() — usar logger.info() com campos estruturados

# AWS
- Dev account: 123456789012
- Prod account: 987654321098
- IAM com least-privilege; sem wildcard resources em Lambda execution roles
```

Com esse arquivo, toda sugestão do Kiro — código, specs, reviews — já segue as convenções do seu projeto. Sem steering files, você corrige as mesmas coisas repetidamente. Com eles, o agent já sabe suas preferências antes de você abrir a boca.

## Hooks: automação em pontos-chave

O sistema de **hooks** permite executar comandos em momentos específicos do ciclo de vida do agent:

- **agentSpawn** — quando o agent inicializa (ex: rodar `git status` para dar contexto)
- **userPromptSubmit** — quando o usuário envia uma mensagem
- **preToolUse** — antes de executar uma ferramenta (pode bloquear a execução!)
- **postToolUse** — depois de executar uma ferramenta
- **stop** — quando o assistente termina de responder

Um exemplo prático: você pode criar um hook `preToolUse` que valida toda escrita de arquivo antes de acontecer, ou um hook `stop` que roda o linter automaticamente depois de cada resposta do Kiro.

```json
{
  "hooks": {
    "preToolUse": [
      {
        "matcher": "fs_write",
        "command": "~/.kiro/hooks/validate-write.sh"
      }
    ],
    "stop": [
      { "command": "cargo fmt --check" }
    ]
  }
}
```

## Knowledge Base: memória persistente com busca semântica

Por padrão, o Kiro só tem acesso ao que está na janela de contexto — os arquivos carregados, o histórico da conversa e as ferramentas. A **knowledge base** expande isso: você indexa arquivos do projeto e o Kiro consegue buscar informação relevante sob demanda, sem precisar carregar tudo no contexto de uma vez.

Na prática, funciona assim: você indexa um diretório de documentação e depois, quando faz uma pergunta, o Kiro busca semanticamente nos arquivos indexados e traz os trechos mais relevantes. É como dar ao assistente uma "memória de longo prazo" sobre o seu projeto.

```
/knowledge add api-docs docs/api/
/knowledge add architecture docs/architecture/
```

Para ativar:

```bash
kiro-cli settings chat.enableKnowledge true
```

Existem dois tipos de índice:
- **Fast** (BM25) — busca por palavras-chave, rápido e leve. Ideal para logs, configs e codebases grandes
- **Best** (all-MiniLM-L6-v2) — busca semântica com embeddings, entende contexto e significado. Ideal para documentação e pesquisa

Cada agent tem sua própria knowledge base **isolada** — o agent de infra pode ter docs da AWS indexados enquanto o agent de backend tem docs da API. O conteúdo persiste entre sessões, então você indexa uma vez e usa sempre.

## MCP Servers: integrações externas

O Kiro suporta **MCP (Model Context Protocol) servers**, que são integrações externas que adicionam ferramentas ao assistente. O ecossistema MCP é compartilhado com outras ferramentas como Claude Code e Cursor, então servidores configurados para uma ferramenta funcionam nas outras.

### MCP Servers da AWS

A AWS mantém mais de 60 MCP servers oficiais no [awslabs/mcp](https://github.com/awslabs/mcp). Alguns dos mais úteis:

```json
{
  "mcpServers": {
    "aws-docs": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"]
    },
    "aws-iac": {
      "command": "uvx",
      "args": ["awslabs.aws-iac-mcp-server@latest"]
    },
    "cfn": {
      "command": "uvx",
      "args": ["awslabs.cfn-mcp-server@latest"]
    }
  }
}
```

- **aws-documentation** — puxa documentação oficial da AWS em tempo real. Útil para serviços que mudam frequentemente
- **aws-iac** — assistência para CDK e CloudFormation: validação de templates, troubleshooting de deploys e best practices
- **cfn** — cria e gerencia mais de 1.100 recursos AWS via Cloud Control API usando linguagem natural

### Terraform MCP Server

O MCP server oficial da HashiCorp conecta o Kiro ao ecossistema Terraform:

```json
{
  "mcpServers": {
    "terraform": {
      "command": "npx",
      "args": ["-y", "terraform-mcp-server@latest"]
    }
  }
}
```

Com ele, o Kiro consulta a documentação de providers e módulos do Terraform Registry em tempo real ao gerar configurações de infraestrutura.

### Git e GitHub

```json
{
  "mcpServers": {
    "git": {
      "command": "mcp-server-git",
      "args": ["--stdio"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "$GITHUB_TOKEN" }
    }
  }
}
```

Suporta tanto servidores locais (stdio) quanto remotos (HTTP), com autenticação OAuth quando necessário.

## Code Intelligence: entendendo o código de verdade

O Kiro tem um sistema de **code intelligence** com duas camadas:

- **Tree-sitter (built-in)** — funciona sem configuração para 18 linguagens. Busca de símbolos com fuzzy matching, pattern search/rewrite via AST e overview do codebase.
- **LSP (opcional)** — com `/code init`, ele conecta language servers para ter find references, go to definition, rename, diagnostics e mais.

Um exemplo poderoso é o **pattern rewrite** via AST. Quer converter todos os `var` para `const` no seu projeto JavaScript?

```
pattern: var $N = $V
replacement: const $N = $V
language: javascript
```

Ele faz isso entendendo a estrutura do código, não com regex frágil.

## Sub-agents: pipelines de trabalho

O Kiro pode orquestrar **múltiplos agents em pipeline** usando o sistema de sub-agents. Você define estágios com dependências e eles executam em paralelo quando possível:

```json
{
  "task": "Add CSV export to the reports page",
  "stages": [
    {"name": "research", "role": "research-agent", "prompt_template": "Gather requirements for {task}"},
    {"name": "implement", "role": "code-agent", "prompt_template": "Implement {task}", "depends_on": ["research"]},
    {"name": "review", "role": "review-agent", "prompt_template": "Review the implementation of {task}", "depends_on": ["implement"]}
  ]
}
```

Isso permite criar fluxos como pesquisa → implementação → revisão, tudo automatizado.

## Skills: conhecimento sob demanda

Os **skills** são recursos que têm seus metadados carregados na inicialização, mas o conteúdo completo só é carregado quando necessário. Isso é útil para ter guias e referências disponíveis sem consumir contexto desnecessariamente.

```markdown
---
name: dynamodb-data-modeling
description: Guide for DynamoDB data modeling best practices. Use when designing or analyzing DynamoDB schema.
---
```

Basta colocar os arquivos em `.kiro/skills/` e referenciá-los na configuração do agent com `skill://`.

## Modelos disponíveis

O Kiro CLI permite trocar de modelo a qualquer momento durante a conversa com `/model`. O picker interativo mostra os modelos disponíveis na sua região junto com o **multiplicador de crédito** de cada um:

- **Claude Sonnet** (ex: claude-sonnet-4) — o modelo padrão, bom equilíbrio entre qualidade e custo (1.0x crédito)
- **Claude Opus** — mais poderoso para raciocínio complexo, mas consome mais (3.0x crédito)
- **Claude Haiku** — mais rápido e barato, ideal para tarefas simples (0.3x crédito)

Você pode definir um modelo padrão para não precisar trocar toda vez:

```bash
kiro-cli settings chat.defaultModel claude-sonnet-4
```

Ou iniciar uma sessão já com um modelo específico:

```bash
kiro-cli chat --model claude-sonnet-4
```

Na IDE, o modo **Auto** roteia automaticamente para o modelo mais custo-efetivo dependendo da complexidade do prompt — tarefas simples vão para um modelo mais leve, raciocínio complexo vai para o Sonnet.

## Economizando tokens e créditos

Tokens são o "combustível" do Kiro — cada mensagem, arquivo de contexto e resposta consome tokens da janela de contexto. Saber gerenciar isso faz diferença no custo e na qualidade das respostas.

### Monitorando o uso

O comando `/context` mostra um breakdown detalhado de como seus tokens estão sendo usados:

```
/context
```

Ele exibe o percentual usado por cada componente: arquivos de contexto, definições de ferramentas, respostas do Kiro, seus prompts e arquivos de sessão. O `/usage` mostra o consumo no nível da conta — quantas interações você já usou do seu plano.

### Compactando o histórico

Conversas longas acumulam muito contexto. O `/compact` gera um resumo da conversa e substitui as mensagens antigas, liberando espaço sem perder informação essencial:

```
/compact
```

Diferente do `/clear` (que apaga tudo), o `/compact` preserva o contexto importante em formato resumido. Use quando o `/context` mostrar que você está chegando perto do limite.

### Dicas práticas para economizar

- **Use steering files** em vez de repetir instruções em todo prompt — eles são carregados uma vez e valem para toda a sessão
- **Skills em vez de resources** — skills (`skill://`) carregam só os metadados na inicialização e o conteúdo completo sob demanda, enquanto resources (`file://`) carregam tudo de uma vez
- **Seja específico nos glob patterns** — `file://src/api/**/*.ts` consome menos contexto que `file://src/**/*`
- **Use Haiku para tarefas simples** — trocar para um modelo mais leve com `/model` em tarefas de baixa complexidade economiza créditos (0.3x vs 1.0x)
- **Remova contexto desnecessário** — `/context remove` tira arquivos que não são mais relevantes para a tarefa atual
- **Comece sessões novas** — `/chat new` para tarefas diferentes evita carregar histórico irrelevante

## Comandos disponíveis

### Comandos CLI (terminal)

Esses são os comandos que você roda direto no terminal:

| Comando | O que faz |
|---------|-----------|
| `kiro-cli chat` | Inicia uma sessão de chat interativa |
| `kiro-cli chat --agent rust-expert` | Inicia com um agent específico |
| `kiro-cli chat --resume` | Retoma a última conversa |
| `kiro-cli chat --no-interactive "query"` | Modo headless para scripts |
| `kiro-cli chat --trust-all-tools` | Auto-aprova todas as ferramentas |
| `kiro-cli agent list` | Lista todos os agents disponíveis |
| `kiro-cli agent create nome` | Cria um novo agent |
| `kiro-cli agent edit nome` | Edita um agent no editor padrão |
| `kiro-cli agent validate --path arquivo` | Valida a configuração de um agent |
| `kiro-cli agent set-default nome` | Define o agent padrão |
| `kiro-cli mcp add --name git --command mcp-server-git` | Adiciona um MCP server |
| `kiro-cli mcp list` | Lista MCP servers configurados |
| `kiro-cli mcp remove --name git` | Remove um MCP server |
| `kiro-cli settings list --all` | Lista todas as configurações disponíveis |
| `kiro-cli settings chat.defaultModel modelo` | Define o modelo padrão |
| `kiro-cli settings --delete chave` | Remove uma configuração |

### Slash commands (dentro do chat)

Esses comandos são usados dentro de uma sessão de chat ativa:

| Comando | O que faz |
|---------|-----------|
| `/help` | Lista todos os slash commands |
| `/agent [nome]` | Troca de agent ou lista disponíveis |
| `/agent create` | Cria um agent interativamente com IA |
| `/model [nome]` | Troca de modelo LLM |
| `/context` | Mostra uso de tokens e gerencia arquivos de contexto |
| `/compact` | Compacta o histórico para liberar contexto |
| `/clear` | Limpa todo o histórico (sem resumo) |
| `/chat save [path]` | Salva a conversa atual |
| `/chat load [path]` | Carrega uma conversa salva |
| `/chat new` | Inicia uma nova sessão |
| `/knowledge` | Gerencia a knowledge base |
| `/code init` | Inicializa code intelligence com LSP |
| `/code overview` | Gera overview da estrutura do codebase |
| `/code summary` | Gera documentação completa do codebase |
| `/code status` | Mostra status dos language servers |
| `/plan` | Muda para o agent de planejamento |
| `/guide [pergunta]` | Muda para o agent guia (ajuda sobre o Kiro) |
| `/tools` | Lista ferramentas disponíveis e gerencia permissões |
| `/hooks` | Mostra hooks configurados |
| `/mcp` | Mostra status dos MCP servers |
| `/usage` | Mostra uso de créditos e plano |
| `/prompts [nome]` | Lista ou seleciona prompts salvos |
| `/paste` | Cola imagem do clipboard |
| `/feedback` | Envia feedback ou reporta problemas |
| `/quit` | Sai do chat |

No TUI também estão disponíveis: `/copy` (copia última resposta), `/editor` (abre o $EDITOR para compor prompt), `/spawn` (cria nova sessão de agent), `/transcript` (abre transcrição no $PAGER) e `/theme` (seleciona tema visual).

## Outros destaques

- **Keyboard shortcuts** — agents podem ter atalhos de teclado (ex: `ctrl+shift+r`) para troca rápida no meio da conversa
- **Welcome messages** — mensagens customizadas ao trocar de agent para orientar o uso
- **Headless mode** — `kiro-cli chat --no-interactive` permite usar o Kiro em scripts e automações CI/CD
- **Sessões por diretório** — conversas são salvas por projeto, então cada repositório tem seu próprio histórico

## Conclusão

O Kiro não é só mais uma ferramenta de IA para código. É um **ecossistema completo** que ataca o problema de desenvolvimento assistido por IA em duas frentes: a **IDE** com spec-driven development para quem quer planejamento estruturado antes de codar, e a **CLI** para quem vive no terminal e quer um assistente que entende profundamente o contexto do projeto.

Ambas compartilham os mesmos conceitos poderosos — agents, steering files, hooks e MCP servers — e a combinação dessas features cria um fluxo de trabalho onde a IA realmente age de forma inteligente, não apenas gera código às cegas.

Se você trabalha com desenvolvimento e ainda não experimentou, vale muito a pena testar. A IDE está disponível em [kiro.dev](https://kiro.dev) com tier gratuito, e a CLI se instala direto no terminal. A curva de aprendizado é suave — você começa com o básico e vai customizando conforme descobre o que precisa.
