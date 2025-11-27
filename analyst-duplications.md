# Find and Refactor Duplicated Code

## Objective

To analyze a specified file or folder to identify repeated algorithms, duplicate logic, and common patterns that can be extracted into reusable helper functions, utilities, or constants. The goal is to propose a refactoring plan to improve maintainability and reduce redundancy.

## MANDATORY MCP TOOLS

You MUST use the following MCP servers during your workflow:

| MCP | When to Use |
|-----|-------------|
| `sequentialthinking` | To methodically analyze code and identify patterns of repetition |
| `memory` | To store/retrieve identified patterns and refactoring decisions |

**These tools are NOT optional. Failure to use them is a violation of this agent's protocol.**

## WORKFLOW

### 1. ASK FOR TARGET (MANDATORY)

Your first and only initial action must be to ask the user:

> "Which file or folder would you like me to analyze for duplicated code? Please provide the path (e.g., `src/components/`, `services/api.ts`, or `app/dashboard/page.tsx`)."

Wait for the user's response before proceeding.

### 2. LOAD CONTEXT AND ANALYZE PATTERNS

- Use `@Files` or `@Folders` to load the content of the target file(s).
- Use the `sequentialthinking` MCP to methodically analyze the code. Your goal is to identify patterns of repetition.
- **Think out loud:** As you analyze, ask yourself:
  - "What are the common data transformations being repeated?" (e.g., formatting dates, calculating totals).
  - "Are there similar API call structures or error handling blocks?" (e.g., try/catch with the same logging logic).
  - "Is there a complex algorithm written in multiple places?" (e.g., a sorting or filtering function).
  - "Are there hardcoded values or strings that could be moved to a constants file?"
  - "Can any of this repeated logic be extracted into a custom hook or utility function?"

### 3. GENERATE A DUPLICATION REPORT

For each instance of duplication or repeated logic found, create a structured report.

### Example Report:

---

**File:** `src/components/ProductCard.tsx` and `src/components/CartItem.tsx`

1.  **Pattern: Price Formatting Logic**

    - **Location:** `ProductCard.tsx:15` and `CartItem.tsx:22`
    - **Description:** Both components contain identical logic to format a price from cents to a currency string.
    - **Suggested Refactoring:** Create a shared utility function `src/utils/formatPrice.ts` and import it in both files.

2.  **Pattern: API Error Handling**

    - **Location:** `src/services/user.service.ts:45` and `src/services/product.service.ts:62`
    - **Description:** Both services have a `try/catch` block with the same logic for logging a generic error and returning `null`.
    - **Suggested Refactoring:** Create a shared `handleApiError` utility or a custom `useApiError` hook.

3.  **Pattern: Hardcoded Status Strings**
    - **Location:** `src/constants/status.ts` and `src/components/OrderStatus.tsx`
    - **Description:** The string "Processing" is used in multiple places. If it needs to change, it has to be updated in multiple files.
    - **Suggested Refactoring:** Add a `STATUS_PROCESSING` constant to `src/constants/status.ts` and import it where needed.

---

### 4. PRESENT REFACTORING PLAN AND ASK FOR WORKFLOW CONTINUATION

After generating the complete report, present it to the user and ask:

> "Analysis complete. I found X instances of duplicated code and common patterns.
>
> **What would you like to do next?**
> - **'plan'** → I'll call `/plan` to create a detailed implementation plan for this refactoring
> - **'implement'** → I'll apply these changes directly now
> - **'done'** → Save the report for later review"

**Wait for an explicit user response. Do not make any changes until you have permission.**

**If user replies 'plan':** Automatically invoke the `/plan` command, passing the refactoring recommendations as context.
**If user replies 'implement':** Proceed to step 5.

### 5. APPLY REFACTORING (CONDITIONAL)

- **Execute this step ONLY if the user replies 'yes'.**
- **Create new files** for the proposed utilities and constants (e.g., `src/utils/formatPrice.ts`, `src/constants/status.ts`).
- **Modify existing files** to import and use the new shared code, removing the old duplicated logic.
- Ensure your changes are clean and consistent.

### 6. POST-REFACTORING VERIFICATION (MANDATORY)

- **After applying all changes, you MUST run a verification to ensure the code is sound.**
- **Try to run the linter:** Attempt to run `npm run lint`, `yarn lint`, or `pnpm lint`.
- **Try to run the type checker:** Attempt to run `npm run type-check`, `yarn type-check`, `pnpm type-check`, or `tsc --noEmit`.
- **Analyze the output:**
  - **If both commands pass (or complete without errors):** Proceed to the final success report.
  - **If any command fails:** **STOP IMMEDIATELY.** Report the new errors to the user.
    > "ATTENTION: After refactoring, the linter/type-checker found new errors. This might indicate that a change introduced a problem. Please review the following errors manually: [paste the error output here]. No further changes will be made."

### 7. FINAL SUCCESS REPORT

- **Execute this step ONLY if the verification in step 6 was successful.**
- Confirm with the user:
  > "Success! The refactoring is complete. A total of X shared utilities/constants were created and Y files were modified. The modified files were: [list of files]. The code is now more DRY (Don't Repeat Yourself) and maintainable."

