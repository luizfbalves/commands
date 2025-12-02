# Fix Linting Issues (Strict Mode)

## Objective

Run the project's linter, identify, and automatically fix all issues, strictly adhering to the following inflexible rules:

- **Absolute Priority:** Fix all **errors** before addressing any **warnings**.
- **GOLDEN RULE (INFLEXIBLE):** It is **STRICTLY FORBIDDEN** to use `eslint-disable`, `// @ts-ignore`, `// @ts-nocheck`, or any similar comment to disable or ignore rules. **Any attempt to use this approach will be considered a complete task failure.** The fix must be in the code, never in the rule.
- **Correction Method:** Apply the canonical, best-practice correction for the language/framework. The solution must solve the root cause of the problem, not just the symptom.

## MANDATORY MCP TOOLS

You MUST use the following MCP servers during your workflow:

| MCP                  | When to Use                                               |
| -------------------- | --------------------------------------------------------- |
| `sequentialthinking` | To analyze lint errors and brainstorm idiomatic solutions |
| `memory`             | To store decisions and lint patterns identified           |

**These tools are NOT optional. Failure to use them is a violation of this agent's protocol.**

## ANTI-HALLUCINATION GUARDRAILS

To prevent hallucinations and ensure factual accuracy, you MUST follow these rules:

| Rule                  | Description                                                |
| --------------------- | ---------------------------------------------------------- |
| **DO NOT invent**     | Never fabricate lint errors not in the actual output       |
| **DO NOT assume**     | Read the actual lint message before proposing a fix        |
| **DO NOT guess**      | If the rule's purpose is unclear, look it up before fixing |
| **ALWAYS cite**       | Quote the exact rule name and error message                |
| **ALWAYS verify**     | Re-run linter after each batch of fixes                    |
| **ALWAYS understand** | Know WHY a rule exists before deciding how to fix          |

**If a rule's purpose is unclear, explicitly state: "I need to understand rule [rule-name] before proposing a fix."**

## Why Disabling Rules is Forbidden?

Disabling rules creates technical debt, masks real code problems, and leads to an inconsistent and hard-to-maintain codebase. Your job is to refactor the code to meet quality standards, not to bypass them.

## Mandatory Thinking Process (Think Out Loud)

Before touching any line of code, you **MUST** follow this process for **EACH** identified linting problem:

1.  **Identify the Problem:** "Problem X was found in file Y at line Z. The rule is `[rule-name]`."
2.  **Analyze the Cause:** "Why was this rule triggered? The code is doing [describe the action], which violates [describe the principle of the rule]."
3.  **Brainstorm Solutions (NO Workarounds):** "What are the possible ways to fix this properly?
    - Option A: [Describe the idiomatic fix]
    - Option B: [Describe another idiomatic fix]
      -..."
4.  **Choose the Best Solution:** "The best approach is Option A because [justify based on clarity, performance, best practices, etc.]."
5.  **Self-Correction:** Check if the chosen solution involves, in any way, disabling a rule. If it does, **STOP** and go back to step 3.

**You must present this analysis for the first 3-5 problems before you start editing the code.** This ensures you are on the right track.

## Workflow

### 1. Run the Linter

- Execute the appropriate lint command for the project (`npm run lint`, `yarn lint`, etc.).

### 2. Analyze and Prioritize

- List all errors and warnings, clearly separating them. Focus 100% on errors first.

### 3. Apply the Mandatory Thinking Process

- For each error, follow the "Think Out Loud" process described above. Present the analysis for the first few problems.

### 4. Execute the Fixes (One by One)

- After analysis and confirmation, apply the planned fix.
- **NEVER** use comments to disable rules.
- If a rule seems impossible to fix, STOP and explain the situation to the user, suggesting a larger code refactoring if necessary.

### 5. Verify and Repeat

- After fixing a batch of issues, run the linter **again**.
- Ensure the errors are resolved and no new issues were introduced.
- Repeat the process for the remaining warnings.

### 6. SELF-VERIFICATION (MANDATORY)

Before declaring success, you MUST:

1. **Run final lint** - confirm zero errors remain
2. **Review all fixes** - ensure no eslint-disable comments were added
3. **Check fix quality** - ensure fixes address root cause, not symptoms
4. **Verify no regressions** - ensure fixes didn't introduce new warnings
5. **Use `memory`** to log: "Fixed X lint errors, Y warnings, no rules disabled"

**Only declare success after completing verification.**

### 7. Final Report

- Provide a clear summary:
  > "Fixing complete. Resolved X errors and Y warnings. Modified files were: [list of files]. Lint now passes without errors and with no rules disabled."

