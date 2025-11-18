# Analisar e Corrigir Interface ou Pasta Next.js 16 (Modo Performance, Verificação e Refatoração)

## Objetivo

Analisar um **arquivo**, **componente** ou **pasta inteira** (incluindo subpastas) do projeto Next.js 16, identificar problemas de desempenho, propor soluções, **com a sua permissão, aplicá-las**, e **verificar se o código permanece sem erros após as mudanças**.

Esse comando também identifica arquivos excessivamente grandes ou pastas com estrutura inadequada e propõe uma reorganização lógica para melhorar a manutenibilidade.

## PASSO 1: PERGUNTAR AO USUÁRIO (OBRIGATÓRIO)

Antes de fazer qualquer coisa, sua primeira e única ação inicial deve ser perguntar ao usuário:

> "Qual interface, componente ou pasta você gostaria que eu analisasse? Por favor, informe o caminho (ex: `components/Header.tsx`, `app/dashboard/page.tsx`, `customer/`, `customer/components/`)."

Aguarde a resposta do usuário antes de prosseguir.

## PASSO 2: LOCALIZAR, CARREGAR E ENTENDER OS ARQUIVOS

- Use `@Files` ou `@Folder` para carregar **todos os arquivos relevantes**, seja um único arquivo ou uma pasta inteira.
- Caso uma pasta seja fornecida, leia recursivamente:
  - Arquivos `.tsx`, `.ts`, `.jsx`, `.js`, `.css`, `.scss`, `.mdx`
  - Subpastas (ex: `customer/components/`)
  - Pages ou layouts relevantes (ex: `customer/page.tsx`, `layout.tsx`)
- Analise como esses arquivos se relacionam entre si:
  - Componentes sendo importados entre si
  - Fluxos de dados entre páginas, hooks e componentes
  - Tamanho, responsabilidade e complexidade geral da estrutura

## PASSO 3: CONSULTAR O MCP SERVER OFICIAL DO NEXT.JS

- Utilize o MCP Server oficial para obter informações atualizadas sobre Next.js 16.
- Exemplos de perguntas recomendadas:
  - "What are the performance best practices for Server Components in Next.js 16?"
  - "When should I use 'use client' and what are the performance implications?"
  - "How to properly optimize images with next/image in Next.js 16?"
  - "What are the common performance pitfalls in Next.js 16 app router?"

## PASSO 4: ANÁLISE DE PERFORMANCE, ARQUITETURA E ESTRUTURA (CHECKLIST)

A análise deve considerar **todos os arquivos encontrados**, aplicando a checklist a seguir:

### Avaliação para cada arquivo ou componente:

- **[ ] Uso de 'use client':** É necessário? Pode ser migrado para Server Component?
- **[ ] Dynamic Imports (next/dynamic):** Arquivos pesados estão sendo carregados de forma estática?
- **[ ] Otimização de Imagens:** Existem `<img>` não substituídos por `<Image>`?
- **[ ] Data Fetching:** Está sendo realizado no local correto (Server Component)? Está usando cache automático?
- **[ ] Estado e Efeitos:** Há `useState`/`useEffect` desnecessários ou que poderiam ser isolados em componentes menores?
- **[ ] Navegação:** Há `<a>` usados incorretamente no lugar de `<Link>`?
- **[ ] Tamanho e Responsabilidade:** Arquivos ou componentes grandes demais?
- **[ ] Estrutura de pasta:** A pasta analisada tem organização coerente? Há módulos desalinhados, responsabilidades misturadas ou uso inadequado de subpastas?

### Avaliação do conjunto da pasta:

- **[ ] Coerência estrutural:** A pasta tem estrutura lógica?
- **[ ] Componentes duplicados ou redundantes:** Há componentes que fazem a mesma função?
- **[ ] Mistura indevida de responsabilidades:** Exemplo: hooks dentro de `/components`, componentes dentro de `/lib`, etc.
- **[ ] Oportunidades de extração para hooks, utils, services ou componentes menores.**

## PASSO 5: GERAR RELATÓRIO E PEDIR PERMISSÃO

Após a análise, gere um relatório **completo**, incluindo:

1. **Problema identificado** e **localização precisa**  
   Exemplo: `customer/components/Card.tsx:44`.

2. **Violação de melhores práticas segundo a documentação do Next.js**  
   Sempre referenciando trechos ou princípios obtidos via MCP quando possível.

3. **Impacto na performance ou arquitetura**  
   Explique claramente o impacto.

4. **Solução proposta**, com diff contendo:
   - Código Antes
   - Código Depois

### Relatório adicional: PROPOSTA DE REESTRUTURAÇÃO DE PASTA (se aplicável)

Se a pasta for desorganizada, ou arquivos muito extensos forem detectados:

---

**PROPOSTA DE REESTRUTURAÇÃO DE PASTA**

**Pasta:** `customer/`

- **Motivo:** A pasta contém:
  - `page.tsx` com mais de 300 linhas
  - Subpastas com componentes misturados entre UI e lógica
  - Hooks e funções utilitárias dentro de `/components`
- **Sugestão de reorganização:**
  1. `customer/hooks/`  
     Para extrair lógica de dados, estados complexos e SWR.
  2. `customer/components/`  
     Apenas componentes puros e UI.
  3. `customer/utils/`  
     Funções auxiliares.
  4. `customer/page.tsx`  
     Reduzido para apenas orquestrar imports.

---

### Pergunta obrigatória ao final:

> "Análise concluída. Foram encontradas X violações de performance e Y oportunidades de refatoração (incluindo Z problemas estruturais na pasta). Deseja que eu aplique todas as correções e, se necessário, reorganize a pasta? Responda **'sim'** para continuar ou **'não'** para cancelar."

**Aguarde permissão explícita.**

## PASSO 6: APLICAR CORREÇÕES E REESTRUTURAR (CONDICIONAL)

Execute apenas se o usuário disser **"sim"**.

- Aplique todas as correções.
- Crie novos arquivos se necessário (para hooks, utils, componentes extraídos).
- Mova arquivos para novas subpastas conforme sugerido.
- Execute linter e type-checker.
- Se houver erros: pare imediatamente e reporte.
- Se estiver tudo correto, confirme:

> "Sucesso! Todas as correções foram aplicadas, reorganizei a estrutura conforme aprovado, e a verificação passou sem erros. Arquivos modificados: [...]. Novos arquivos criados: [...]."
