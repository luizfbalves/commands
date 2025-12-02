# Command: /planner-stories

Automatically generates a complete user story specification based on the established project pattern.

## MANDATORY MCP TOOLS

You MUST use the following MCP servers during your workflow:

| MCP | When to Use |
|-----|-------------|
| `sequentialthinking` | To structure the story generation and analyze existing patterns |
| `memory` | To store project context and story conventions |

**These tools are NOT optional. Failure to use them is a violation of this agent's protocol.**

## ANTI-HALLUCINATION GUARDRAILS

To prevent hallucinations and ensure factual accuracy, you MUST follow these rules:

| Rule | Description |
|------|-------------|
| **DO NOT invent** | Never fabricate project patterns not visible in existing specs |
| **DO NOT assume** | Read actual templates before generating content |
| **DO NOT guess** | If template structure is unclear, ask the user |
| **ALWAYS follow** | Base all generated content on existing patterns in `specs/` |
| **ALWAYS verify** | Check that generated files match the template format exactly |
| **ALWAYS contextualize** | Use actual project stack (NestJS, Next.js, etc.) in requirements |

**If template files cannot be found, explicitly state: "I cannot find the template files. Please verify the `specs/.template/` folder exists."**

## Usage

```
/planner-stories [feature-name]
```

## 📝 Examples

```
/planner-stories login-flow
/planner-stories push-notification-system
/planner-stories sales-dashboard-and-metrics
/planner-stories payment-gateway-integration
/planner-stories reviews-and-comments-system
```

## ✨ What It Does

When you use `/planner-stories [feature-name]`, the assistant will:

1. **Analyze existing pattern:**

   - Read `specs/.template/` to understand the format
   - Analyze `specs/2025-11-15-onboarding-flow/` as reference
   - Check `specs/README.md` for expected structure

2. **Create complete structure:**

   - Execute `./scripts/create-spec.sh [feature-name]`
   - Create directory `specs/YYYY-MM-DD-[feature-name]/`

3. **Fill all files:**
   - ✅ `user-story.md` - Complete user story with scenarios
   - ✅ `requirements.md` - Detailed technical requirements
   - ✅ `api-contracts.md` - API contracts (if applicable)
   - ✅ `database-changes.md` - Database changes (if applicable)
   - ✅ `implementation-notes.md` - Checklist and notes
   - ✅ `README.md` - Feature overview

## 📋 Generated Content

### User Story (`user-story.md`)

- Format: "As [persona], I want [action], So that [benefit]"
- Detailed functionality description
- Specific and testable acceptance criteria
- 3-5 usage scenarios (Given/When/Then format)
- Dependencies and limitations

### Requirements (`requirements.md`)

- Affected components (frontend, backend, database)
- Functional requirements with priority and complexity
- Non-functional requirements (performance, security, usability)
- Complete data flow
- Testing strategy

### API Contracts (`api-contracts.md`)

- Endpoints with complete request/response
- Request examples (cURL and TypeScript)
- Required authentication/authorization
- Error codes and validations

### Database Changes (`database-changes.md`)

- New tables or modifications
- Relationships and indexes
- Necessary migrations
- Integrity considerations

### Implementation Notes (`implementation-notes.md`)

- Implementation checklist
- Structure for technical decisions
- Sections for encountered problems
- Future improvements

## 🎯 Pattern Followed

The command follows the pattern established in:

- 📁 `specs/.template/` - Base template with all files
- 📁 `specs/2025-11-15-onboarding-flow/` - Complete reference example
- 📄 `specs/README.md` - Pattern documentation and usage guide

## 💡 Usage Tips

- **Be specific:** "login-flow-with-2fa" is better than "login"
- **Use descriptive names:** "push-notification-system" is better than "notifications"
- **Context helps:** If the feature is complex, the assistant may ask questions
- **Detailing:** Content is specific and detailed, not generic

## 🔧 Project Context

The generator automatically considers:

- Monorepo: `apps/api` (NestJS) + `apps/web` (Next.js)
- Database: PostgreSQL with Prisma
- Auth: Supabase Auth (JWT)
- Storage: Supabase Storage
- State: Zustand
- Forms: React Hook Form + Zod
- Tests: Unit, integration, and E2E

## 📚 Complete Example

**Command:**

```
/planner-stories login-flow
```

**Result:**

```
specs/2025-11-15-login-flow/
├── README.md                    # Overview
├── user-story.md                # Complete user story
├── requirements.md              # Technical requirements
├── api-contracts.md             # Auth endpoints
├── database-changes.md          # (if there are changes)
└── implementation-notes.md      # Checklist
```

All files are automatically filled with relevant and specific content for the requested feature.

**Use `sequentialthinking` and `memory` MCP tools throughout the workflow.**

## SELF-VERIFICATION (MANDATORY)

Before creating any files, you MUST:

1. **Verify template exists** - confirm `specs/.template/` is accessible
2. **Compare with reference** - ensure generated content matches `specs/2025-11-15-onboarding-flow/` format
3. **Check project context** - verify tech stack references are accurate
4. **Validate scenarios** - ensure Given/When/Then format is correctly applied
5. **Use `memory`** to log: "Verified template format, generated X files following pattern"

**Only create files after completing verification.**

