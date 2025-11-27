# Fix TypeScript Type Errors (Iterative & Idiomatic Mode)

## Objective

To run the project's type-checker, identify TypeScript errors (including `any` types), and fix them using the best possible, most idiomatic TypeScript approach. This process repeats iteratively until the project passes all type checks with no `any` types remaining.

## MANDATORY MCP TOOLS

You MUST use the following MCP servers during your workflow:

| MCP | When to Use |
|-----|-------------|
| `sequentialthinking` | To decompose type problems and analyze potential solutions |
| `memory` | To store important decisions made during type fixing |
| `context7` | To get documentation for libraries and TypeScript patterns |
| `shadcn` | When fixing types related to UI components |
| `next-devtools` | When fixing types related to Next.js code (Server Components, routes, etc.) |

**These tools are NOT optional. Failure to use them is a violation of this agent's protocol.**

## GOLDEN RULE (INFLEXIBLE)

- **STRICTLY FORBIDDEN:** Using `any`, `unknown` (as a lazy replacement), `@ts-ignore`, `@ts-nocheck`, or type assertions (`as any`) to silence errors.
- **MANDATORY APPROACH:** You MUST understand the root cause of the type error and fix it with a robust, type-safe solution. You must derive the correct type from schemas, existing types, or the project's logic.

## WORKFLOW

### 1. IDENTIFY SCOPE AND RUN THE TYPE-CHECK COMMAND

- If the user provides a path after the command (e.g., `/fixer-types src/components/`), run the type-check only for that path.
- If no path is provided, run the type-check for the **entire project**.
- The command to be executed is **`bun run type-check`**.

### 2. RUN THE CHECK AND PARSE THE OUTPUT

- Execute `bun run type-check` and capture the full output.
- Identify the **first error** reported by the compiler (TypeScript errors or `any` type occurrences).

### 3. MANDATORY THINKING PROCESS (Think Out Loud)

Before making any changes, you **MUST** follow this process for the identified error:

#### Step 0: Use MANDATORY MCP Tools

Before analyzing the specific type issue, you **MUST** use the MCP tools defined in the MANDATORY MCP TOOLS section above to gain deep understanding of context and make informed decisions.

#### Step 1: Identify the Error

"The error is `[paste the exact TS error code and message]` in file `[file:line]` for variable/parameter `[variableName]`."

#### Step 2: Analyze the Root Cause

"Why is TypeScript complaining? The underlying issue is [explain the technical reason in detail. e.g., 'The API response type was not defined', 'The function can accept multiple formats', 'The library type definitions are incomplete', 'A variable is being used before it's assigned a value in all code paths']."

#### Step 3: Brainstorm Idiomatic Solutions (No Shortcuts)

"What are the best ways to fix this?
- **Solution A (Infer from Schema/Type):** [Describe a solution using a Zod schema, database model type, or existing interface. e.g., 'The API response should be typed as `User` from `src/types/user.ts`.]
- **Solution B (Create a Union Type):** [Describe a solution using a union of possible types if the value can be one of several known types. e.g., 'The prop can be a `string` or `number`, so it should be `string | number`.]
- **Solution C (Use a Generic):** [Describe a solution using a generic type if the exact format is unknown but has a known structure. e.g., 'The function should accept a generic `T` that extends `{ id: string }`.]
- **Solution D (Type Guard):** [Describe a solution using a runtime check to narrow the type. e.g., 'Use `if (isUser(data)) { ... }` to narrow `unknown` to `User`.]
- **Solution E (Default Value):** [Describe a solution using nullish coalescing or default parameters. e.g., `const name = user.name ?? 'Default Name';`]
- **Solution F (Refactor Type):** [Describe a solution that fixes the underlying type definition. e.g., 'Make the `name` property required in the `User` interface'.]"

#### Step 4: Choose the Best Solution and Justify

"The best approach is **Solution A**, because [justify based on type safety, readability, and avoiding runtime errors]. This makes the code more predictable and self-documenting."

**You must present this complete analysis, including MCP insights, for the first error before making any edits.**

### 4. APPLY THE FIX

- After the analysis, apply the chosen solution to the code.
- **DO NOT** use `any`, `unknown`, or type assertions unless it is an absolute last resort for an unsolvable third-party library issue, and you must justify why.

### 5. RE-RUN THE CHECK AND REPEAT

- **This step is crucial.** Run `bun run type-check` **again**.
- If it still fails or finds another type issue, go back to step 2 and analyze the new first error.
- Continue this cycle until the type-check command completes successfully with no errors and no `any` types.

### 6. FINAL SUCCESS REPORT

- Once the type-check passes with no `any` types, confirm with the user:
  > "Success! The project now passes all type checks with no `any` types remaining. A total of X type issues were fixed across Y files. The modified files were: [list of files]. The code is now fully type-safe and robust."

