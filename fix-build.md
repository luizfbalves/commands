# Fix Build Errors (Iterative & Strict Mode)

## Objective

Run the project's build, identify the **first** compilation error, fix it, and repeat this process iteratively until the build succeeds.

## GOLDEN RULE (INFLEXIBLE)

It is **STRICTLY FORBIDDEN** to:

- Comment out entire blocks of code to silence errors.
- Remove functional code or business logic.
- Use `// @ts-ignore`, `// @ts-nocheck`, or any similar directive to ignore type or compilation errors.

**The fix must resolve the root cause of the build problem, not just hide it.** Any attempt to bypass this rule will be considered a complete task failure.

## Mandatory Thinking Process (Think Out Loud)

Before making any changes, you **MUST** follow this process for **EACH** build error found:

1.  **Identify the Error:** "The build failed with the error: `[paste the exact error message]`. The problem is located in `[file:line]`."
2.  **Analyze the Root Cause:** "Why is this error happening? The fundamental cause is [explain the technical cause in detail. e.g., 'type 'string' is not assignable to the variable expecting a 'number'', 'module 'react-router-dom' could not be found', 'a closing '}' brace is missing for the previous code block']."
3.  **Brainstorm Idiomatic Solutions:** "What are the correct ways to fix this?
    - **Option A:** [Describe the most common and idiomatic fix. e.g., 'Add the missing module import at the top of the file.']
    - **Option B:** [Describe an alternative. e.g., 'Change the variable's type to match what is being assigned.']
    - **Option C:** [Describe another alternative. e.g., 'Add the missing syntax to close the code block.']"
4.  **Choose the Best Solution and Justify:** "The best approach is **Option A**, because [justify based on best practices, code clarity, or because it's the most direct and least impactful solution]."

**You must present this full analysis for the first error before making any edits.**

## Iterative Workflow

1.  **Identify the Build Command:** Try common commands like `npm run build`, `yarn build`, `pnpm build`, `mvn compile`, `cargo build`, etc. If not found, ask the user for the correct command.

2.  **Run the Build:** Execute the build command and capture the full output.

3.  **Analyze the First Error:** Focus **only on the first reported error**. Build tools often stop at the first critical error, so fixing them sequentially is the most efficient approach.

4.  **Apply the Thinking Process:** Follow the "Think Out Loud" process for the identified error.

5.  **Apply the Fix:** After the analysis, make the necessary code change.

6.  **Re-run the Build:** **This step is crucial.** Run the build command **again**.

7.  **Repeat:** If the build still fails, go back to step 3 and analyze the new first error. Continue this cycle until the build runs without errors.

8.  **Final Report:** After the build succeeds, present a summary:
    > "Build completed successfully after X correction cycles. A total of Y errors were fixed. The modified files were: [list of files]."
