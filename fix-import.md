# Corrigir Imports para Paths Relativos (TypeScript)

## Objetivo

Encontrar imports que usam caminhos relativos complexos (ex: `../../../utils/funcao.tsx`) e substituí-los por paths de alias configurados no `tsconfig.json` do projeto, resultando em imports mais limpos e manuteníveis (ex: `@/utils/funcao.tsx`).

## PASSO 1: PERGUNTAR AO USUÁRIO (OBRIGATÓRIO)

Sua primeira e única ação inicial deve ser perguntar ao usuário:

> "Qual arquivo ou pasta você gostaria que eu revisasse para corrigir os imports? Por favor, me forneça o caminho (ex: `components/`, `src/services/user.service.ts`, ou `app/dashboard/`)."

Aguarde a resposta do usuário antes de prosseguir.

## PASSO 2: LER E ENTENDER O `tsconfig.json`

- **Antes de analisar qualquer arquivo, use `@Files` para carregar o `tsconfig.json` na raiz do projeto.**
- Analise a seção `compilerOptions.paths` para identificar todos os aliases de caminho disponíveis (ex: `@/*`, `@components/*`, `@lib/*`).
- Crie um mapa mental destes aliases para usar nas substituições.

## PASSO 3: ANALISAR ARQUIVOS E IDENTIFICAR IMPORTS PROBLEMÁTICOS

- Use o símbolo `@Files` ou `@Folders` para carregar o conteúdo do arquivo ou de todos os arquivos na pasta especificada.
- Procure por todas as declarações `import` que usem caminhos relativos com múltiplos níveis (ex: `../..`, `../../..`).
- **Ignore imports simples** como `./Component` ou `../types`, pois são geralmente aceitáveis.

## PASSO 4: MAPEAR PARA O ALIAS CORRETO E GERAR RELATÓRIO

Para cada import problemático encontrado:

1.  **Calcule o caminho absoluto** a partir da raiz do projeto.
2.  **Verifique se há um alias** no `tsconfig.json` que corresponda a este caminho absoluto.
3.  **Se houver um alias correspondente**, proponha a substituição.

### Exemplo de Relatório:

---

**Arquivo:** `src/components/forms/UserForm.tsx`

1.  **Linha 5:** `import { validateUser } from '../../../utils/validation';`

    - **Ação:** **SUBSTITUIR**
    - **Justificativa:** O caminho relativo é longo e frágil. Usando o alias `@/` definido no `tsconfig.json`, o import se torna mais limpo e resistente a mudanças na estrutura de pastas.
    - **Proposta:**
      ```diff
      - import { validateUser } from '../../../utils/validation';
      + import { validateUser } from '@/utils/validation';
      ```

2.  **Linha 8:** `import { Button } from '../Button';`

    - **Ação:** **MANTER**
    - **Justificativa:** Import relativo simples e local. A substituição não traria benefícios significativos.

3.  **Linha 12:** `import { api } from '../../../../services/api';`
    - **Ação:** **SUBSTITUIR**
    - **Justificativa:** Caminho relativo excessivamente complexo.
    - **Proposta:**
      ```diff
      - import { api } from '../../../../services/api';
      + import { api } from '@/services/api';
      ```

---

## PERGUNTA FINAL OBRIGATÓRIA

Após apresentar o relatório completo, pergunte:

> "Análise concluída. Encontrei X imports para substituir por paths de alias. Deseja que eu aplique todas as correções sugeridas? Por favor, responda **'sim'** para prosseguir ou **'não'** para cancelar."

**Aguarde uma resposta explícita do usuário. Não faça nenhuma alteração até receber a permissão.**

## PASSO 5: APLICAR CORREÇÕES E VERIFICAR

- **Execute este passo APENAS se o usuário responder 'sim'.**
- Aplique cada uma das substituições propostas no relatório.
- \*\*Após aplicar as mudanças, execute o linter e o type-checker (`tsc --noEmit`) para garantir que os paths estão corretos e o código compila.
