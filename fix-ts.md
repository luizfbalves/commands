# Fix TypeScript Errors (Iterative & Idiomatic Mode)

## Objective

To run the project's type-checker, identify the **first** TypeScript error, and fix it using the best possible, most idiomatic TypeScript approach. This process repeats until the project passes all type checks.

## GOLDEN RULE (INFLEXIBLE)

- **STRICTLY FORBIDDEN:** Using `any`, `@ts-ignore`, or `@ts-nocheck` as a lazy way to silence errors.
- **MANDATORY APPROACH:** You MUST understand the root cause of the type error and fix it with a robust, type-safe solution. Prefer refactoring types, adding proper type guards, or providing default values over type assertions.

## WORKFLOW

### 1. IDENTIFY SCOPE AND RUN THE TYPE-CHECK COMMAND

- The command to be executed is **`bun run type-check`**.
- If the user provided a path after the command (e.g., `/fix-typescript-errors src/components/`), run `bun run type-check` only for that path.
- If no path was provided, run `bun run type-check` for the **entire project**.

### 2. RUN THE CHECK AND PARSE THE OUTPUT

- Execute `bun run type-check` and capture the full output.
- Identify the **first error** reported by the compiler. TypeScript typically stops at the first error that blocks compilation.

### 3. MANDATORY THINKING PROCESS (Think Out Loud)

Before making any changes, you **MUST** follow this process for the identified error:

1.  **Identify the Error:** "The error is `[paste the exact TS error code and message]` in file `[file:line]`."
2.  **Analyze the Root Cause:** "Why is TypeScript complaining? The underlying issue is [explain the technical reason in detail. e.g., 'A variable is being used before it's assigned a value in all code paths', 'A function is being called with an argument of an incompatible type', 'An object property might be undefined']."
3.  **Brainstorm Idiomatic Solutions (No Shortcuts):** "What are the best ways to fix this?
    - **Solution A (Type Guard):** [Describe a solution using `if` checks or type predicates. e.g., `if (user && user.name) { ... }`]
    - **Solution B (Default Value):** [Describe a solution using nullish coalescing or default parameters. e.g., `const name = user.name ?? 'Default Name';`]
    - **Solution C (Refactor Type):** [Describe a solution that fixes the underlying type definition. e.g., 'Make the `name` property required in the `User` interface'.]
    - **Solution D (Utility Type):** [Describe a solution using advanced utility types if applicable. e.g., `Extract<...>` or `Omit<...>`]"
4.  **Choose the Best Solution and Justify:** "The best approach is **Solution A**, because [justify based on robustness, readability, and avoiding runtime errors]."

**You must present this analysis for the first error before making any edits.**

### 4. APPLY THE FIX

- After the analysis, apply the chosen solution to the code.
- **DO NOT** use `any` or `@ts-ignore` unless it is an absolute last resort for an unsolvable third-party library issue, and you must justify why.

### 5. RE-RUN THE CHECK AND REPEAT

- **This step is crucial.** Run `bun run type-check` **again**.
- If it still fails, go back to step 2 and analyze the new first error.
- Continue this cycle until the type-check command completes successfully without any errors.

### 6. FINAL SUCCESS REPORT

- Once the type-check passes, confirm with the user:
  > "Success! The project now passes all type checks. A total of X errors were fixed across Y files. The modified files were: [list of files]. The code is now type-safe and robust."
