# Comando: /storie

Gera automaticamente uma especificação completa de user story baseada no padrão estabelecido no projeto.

## 🚀 Uso

```
/storie [nome-da-feature]
```

## 📝 Exemplos

```
/storie fluxo de login
/storie sistema de notificações push
/storie dashboard de vendas e métricas
/storie integração com gateway de pagamento
/storie sistema de avaliações e comentários
```

## ✨ O que faz

Quando você usar `/storie [nome-da-feature]`, o assistente irá:

1. **Analisar o padrão existente:**

   - Ler `specs/.template/` para entender o formato
   - Analisar `specs/2025-11-15-fluxo-onboarding/` como referência
   - Verificar `specs/README.md` para estrutura esperada

2. **Criar estrutura completa:**

   - Executar `./scripts/create-spec.sh [nome-da-feature]`
   - Criar diretório `specs/YYYY-MM-DD-[nome-da-feature]/`

3. **Preencher todos os arquivos:**
   - ✅ `user-story.md` - User story completa com cenários
   - ✅ `requirements.md` - Requisitos técnicos detalhados
   - ✅ `api-contracts.md` - Contratos de API (se aplicável)
   - ✅ `database-changes.md` - Mudanças no banco (se aplicável)
   - ✅ `implementation-notes.md` - Checklist e notas
   - ✅ `README.md` - Visão geral da feature

## 📋 Conteúdo Gerado

### User Story (`user-story.md`)

- Formato: "Como [persona], Quero [ação], Para que [benefício]"
- Descrição detalhada da funcionalidade
- Critérios de aceitação específicos e testáveis
- 3-5 cenários de uso (formato Given/When/Then)
- Dependências e limitações

### Requirements (`requirements.md`)

- Componentes afetados (frontend, backend, banco)
- Requisitos funcionais com prioridade e complexidade
- Requisitos não-funcionais (performance, segurança, usabilidade)
- Fluxo de dados completo
- Estratégia de testes

### API Contracts (`api-contracts.md`)

- Endpoints com request/response completos
- Exemplos de requisições (cURL e TypeScript)
- Autenticação/autorização necessária
- Códigos de erro e validações

### Database Changes (`database-changes.md`)

- Novas tabelas ou modificações
- Relacionamentos e índices
- Migrações necessárias
- Considerações de integridade

### Implementation Notes (`implementation-notes.md`)

- Checklist de implementação
- Estrutura para decisões técnicas
- Seções para problemas encontrados
- Melhorias futuras

## 🎯 Padrão Seguido

O comando segue o padrão estabelecido em:

- 📁 `specs/.template/` - Template base com todos os arquivos
- 📁 `specs/2025-11-15-fluxo-onboarding/` - Exemplo completo de referência
- 📄 `specs/README.md` - Documentação do padrão e guia de uso

## 💡 Dicas de Uso

- **Seja específico:** "fluxo de login com 2FA" é melhor que "login"
- **Use nomes descritivos:** "sistema de notificações push" é melhor que "notificações"
- **Contexto ajuda:** Se a feature for complexa, o assistente pode fazer perguntas
- **Português brasileiro:** Todo conteúdo é gerado em pt-BR
- **Detalhamento:** O conteúdo é específico e detalhado, não genérico

## 🔧 Contexto do Projeto

O gerador considera automaticamente:

- Monorepo: `apps/api` (NestJS) + `apps/web` (Next.js)
- Banco: PostgreSQL com Prisma
- Auth: Supabase Auth (JWT)
- Storage: Supabase Storage
- Estado: Zustand
- Formulários: React Hook Form + Zod
- Testes: Unitários, integração e E2E

## 📚 Exemplo Completo

**Comando:**

```
/storie fluxo de login
```

**Resultado:**

```
specs/2025-11-15-fluxo-login/
├── README.md                    # Visão geral
├── user-story.md                # User story completa
├── requirements.md              # Requisitos técnicos
├── api-contracts.md             # Endpoints de auth
├── database-changes.md          # (se houver mudanças)
└── implementation-notes.md      # Checklist
```

Todos os arquivos são preenchidos automaticamente com conteúdo relevante e específico para a feature solicitada.

**use pensamento sequencial**
