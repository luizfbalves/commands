# Corrigir Tipos `any` (Modo Idiomático e Seguro)

## Objetivo

Executar o type-checker do projeto, identificar a **primeira** ocorrência do tipo `any` e corrigi-la usando a melhor abordagem possível, mais específica e idiomática do TypeScript. Este processo se repete até que o projeto esteja livre de tipos `any`.

## REGRA DE OURO (INFLEXÍVEL)

- **ESTRITAMENTE PROIBIDO:** Substituir um `any` por outro `any` (ex: `unknown`) ou usar asserções de tipo (`as any`) como uma correção preguiçosa.
- **ABORDAGEM OBRIGATÓRIA:** Você PRECISA entender a causa raiz do tipo `any` e corrigi-la com uma solução robusta e type-safe. Você deve derivar o tipo correto a partir dos schemas, tipos existentes ou da lógica do projeto.

## FLUXO DE TRABALHO

### 1. IDENTIFICAR O ESCOPO E EXECUTAR O COMANDO DE TYPE-CHECK

- Se o usuário fornecer um caminho após o comando (ex: `/fix-any-types src/components/`), execute o type-check apenas para esse caminho.
- Se nenhum caminho for fornecido, execute o type-check para o **projeto inteiro**.
- O comando a ser executado é **`bun run type-check`**.

### 2. EXECUTAR O CHECK E PARAR A SAÍDA

- Execute `bun run type-check` e capture a saída completa.
- Identifique a **primeira ocorrência do tipo `any`** reportada pelo compilador.

### 3. PROCESSO DE PENSAMENTO OBRIGATÓRIO (Pense em Voz Alta)

Antes de fazer qualquer alteração, você **DEVE** seguir este processo para o tipo `any` identificado:

#### Passo 0: Usar os MCPs Disponíveis

Antes de começar a analisar o tipo `any` específico, você **DEVE** usar os MCPs disponíveis para obter um entendimento profundo do contexto e tomar decisões informadas.

- Use `sequentialthinking` para decompor o problema e analisar soluções em potencial.
- Use `memory` para armazenar decisões importantes tomadas durante a análise.
- Use `context7` para obter uma visão geral da arquitetura do projeto e dos padrões existentes.
- Use `shadcn` ou `nextjs` MCPs se o código a ser corrigido estiver relacionado a componentes de UI ou a padrões do Next.js.

#### Passo 1: Identificar o Erro

"O tipo `any` está sendo usado em `[arquivo:linha]` para a variável/parâmetro `[nomeDaVariavel]`."

#### Passo 2: Analisar a Causa Raiz

"Por que o `any` foi usado aqui? A razão subjacente é [explique o motivo técnico em detalhes. ex: 'O tipo da resposta da API não estava definido', 'A função pode aceitar múltiplos formatos', 'As definições de tipo da biblioteca estão incompletas']."

#### Passo 3: Brainstorm de Soluções Idiomáticas (Sem Atalhos)

"Qual é a melhor maneira de corrigir isso? - **Solução A (Inferir a partir de Schema/Tipo):** [Descreva uma solução usando um schema Zod, um tipo de modelo de banco de dados ou uma interface existente. ex: 'A resposta da API deveria ser tipada como `User` de `src/types/user.ts`.] - **Solução B (Criar um Tipo de União):** [Descreva uma solução usando uma união de tipos possíveis se o valor pode ser um de vários tipos conhecidos. ex: 'A prop pode ser uma `string` ou `number`, então deveria ser `string | number`.] - **Solução C (Usar um Genérico):** [Descreva uma solução usando um tipo genérico se o formato exato é desconhecido, mas tem uma estrutura conhecida. ex: 'A função deveria aceitar um genérico `T` que extende `{ id: string }`.] - **Solução D (Type Guard):** [Descreva uma solução usando uma verificação em tempo de execução para estreitar o tipo. ex: 'Use `if (isUser(data)) { ... }` para estreitar `unknown` para `User`.]"

#### Passo 4: Escolher a Melhor Solução e Justificar

"A melhor abordagem é a **Solução A**, porque [justifique com base em segurança de tipo, legibilidade e para evitar erros em tempo de execução. Isso torna o código mais previsível e autodocumentado."

**Você deve apresentar esta análise completa, incluindo os insights dos MCPs, para o primeiro `any` antes de fazer qualquer edição.**

### 4. APLICAR A CORREÇÃO

- Após a análise, aplique a solução escolhida ao código.
- **NÃO** use `any`, `unknown` ou asserções de tipo, a menos que seja o último recurso para um problema de biblioteca de terceiros insolúvel, e você deve justificar o porquê.

### 5. EXECUTAR O CHECK NOVAMENTE E REPETIR

- **Este passo é crucial.** Execute `bun run type-check` **novamente**.
- Se ainda falhar e encontrar outro `any`, volte ao passo 2 e analise a nova primeira ocorrência.
- Continue este ciclo até que o comando de type-check seja concluído com sucesso, sem nenhum tipo `any`.

### 6. RELATÓRIO FINAL DE SUCESSO

- Uma vez que o type-check passe sem tipos `any`, confirme com o usuário:
  > "Sucesso! O projeto agora está livre de tipos `any`. Um total de X tipos `any` foram corrigidos em Y arquivos. Os arquivos modificados foram: [lista de arquivos]. O código agora está mais type-safe e robusto."
