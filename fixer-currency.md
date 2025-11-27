# Review Monetary Standard (Cents Pattern)

## Objective

Analyze files or folders to ensure that monetary value handling strictly follows the **cents (integers)** pattern, as per the provided documentation. The goal is to find inconsistencies, bad practices, and potential bugs related to price manipulation.

## MANDATORY MCP TOOLS

You MUST use the following MCP servers during your workflow:

| MCP | When to Use |
|-----|-------------|
| `memory` | To store monetary pattern decisions and identified violations |
| `mercadopago-mcp-server` | To validate payment patterns and get best practices for payment integration |

**These tools are NOT optional. Failure to use them is a violation of this agent's protocol.**

## REFERENCE DOCUMENTATION (SOURCE OF TRUTH)

Use the following documentation as the sole source of truth for this analysis:

**FUNDAMENTAL RULE:** The entire system uses the cents pattern to store and transmit monetary values (API, Database, WebSocket). Conversion to currency decimals happens **ONLY** in the frontend for display purposes.

- **API/Backend:** Sends/receives `price: 3590` (representing $35.90).
- **Database:** Stores `3590` (INTEGER).
- **Frontend:** Receives `3590`, uses utilities like `centsToDecimal` or `<FormattedPrice>` to display "$35.90".
- **Forms:** User types "35.90" -> Frontend converts to `3590` -> Sends to API.

**PRIMARY UTILITY:** For all frontend price formatting and conversions, you **MUST** use the utility functions located at `apps/web/lib/utils/price.ts`. This is the standard way to handle price display and manipulation in the web app.

## STEP 1: ASK THE USER (MANDATORY)

Your first and only initial action must be to ask the user:

> "Which file or folder would you like me to review regarding the monetary standard? Please provide the path (e.g., `services/payment.service.ts`, `components/ProductCard.tsx`, or `src/app/products/`)."

Wait for the user's response before proceeding.

## STEP 2: LOAD AND ANALYZE THE CODE

- Use the `@Files` or `@Folders` symbol to load the content of the file or all files in the specified folder.
- Analyze the code line by line, looking for violations of the documented pattern.

## STEP 3: MANDATORY ANALYSIS CHECKLIST

For each file, systematically check the following points:

- **[ ] Decimal Storage/Transmission:** Is the code storing or sending prices in decimal format (e.g., `35.90`) instead of cents (e.g., `3590`)? Look for numeric literals with decimal places assigned to price variables.
- **[ ] Calculations with Decimals:** Are mathematical operations (`+`, `-`, `*`, `/`) being performed with values in decimals instead of cents? This violates the precision principle.
- **[ ] Direct Value Display:** Is the value received from the API (in cents) being displayed directly to the user without formatting (e.g., `<span>{price}</span>` displays "3590")?
- **[ ] Form Input Handling:** Is a price input's value being sent to the API as a string or decimal without the `decimalToCents` conversion?
- **[ ] Lack of Validation:** Are price values being received (from API, input, etc.) without validation using `isValidPrice` or `getSafePrice`?
- **[ ] WebSocket Inconsistency:** Are WebSocket messages emitting or receiving prices in decimal format?
- **[ ] Incorrect Utility Usage:** Is the code using `centsToDecimal` to send data to the API (error) or trying to format a value that is already in decimals (logical error)?

## STEP 4: GENERATE REPORT AND ASK FOR PERMISSION

After the analysis, generate a structured report. For each issue found, you MUST:

1. **Identify the Problem and Location:** "Problem: Total calculation with decimal values in file `cart-summary.tsx`, line 15."
2. **Point out the Documentation Violation:** "Violation: This breaks the rule 'All calculations must be done in cents to ensure precision'."
3. **Explain the Impact:** "Impact: This can result in rounding inaccuracies and incorrect cart totals, leading to financial and user trust issues."
4. **Propose the Solution (Code):** Provide a clear diff with "Before Code" and "After Code".

### **Formatting Rule:**

When proposing a fix for value display or conversion, you **MUST** use the functions from `apps/web/lib/utils/price.ts`. Do not implement manual formatting (e.g., `(price / 100).toFixed(2)`). Instead, use the provided utility functions like `formatPrice` or `centsToDecimal` from that specific file.

### Example of a Proposed Solution:

```diff
// cart-summary.tsx

// ...
- const total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
+ const total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
+ // Note: item.price is already in cents, so the calculation is direct and precise.

// ...
- <span>Total: $ {total}</span>
+ <span>{formatPrice(total)}</span>
+ // Uses the centralized formatPrice utility from apps/web/lib/utils/price.ts for consistent display.
```

## FINAL MANDATORY QUESTION

After presenting the complete report, ask:

> "Analysis complete. I found X violations related to the monetary pattern.
>
> **What would you like to do next?**
> - **'plan'** → I'll call `/plan` to create a detailed implementation plan for these fixes
> - **'implement'** → I'll apply all corrections directly now
> - **'done'** → Save the report for later review"

**Wait for an explicit user response. Do not make any changes until you have permission.**

**If user replies 'plan':** Automatically invoke the `/plan` command, passing the monetary pattern fixes as context.
**If user replies 'implement':** Proceed with applying the corrections.

