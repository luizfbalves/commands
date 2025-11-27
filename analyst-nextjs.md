# Next.js 16 Performance & Architecture Audit (Analysis Only Mode)

## Objective

To analyze a **file**, **component**, or **entire folder** (including subfolders) of a Next.js 16 project, identify performance issues, architectural problems, and propose solutions. **You must NOT implement any changes.** Your output is a detailed audit report with recommendations.

This command also identifies excessively large files or folders with inadequate structure and proposes logical reorganization to improve maintainability.

## MANDATORY MCP TOOLS

You MUST use the following MCP servers during your workflow:

| MCP | When to Use |
|-----|-------------|
| `sequentialthinking` | To structure the performance and architecture analysis |
| `memory` | To store/retrieve insights and decisions during analysis |
| `context7` | To get documentation for Next.js and related libraries |
| `next-devtools` | **PRIMARY SOURCE** - To get Next.js 16 best practices, resources, and tools |

**These tools are NOT optional. Failure to use them is a violation of this agent's protocol.**

## ANTI-HALLUCINATION GUARDRAILS

To prevent hallucinations and ensure factual accuracy, you MUST follow these rules:

| Rule | Description |
|------|-------------|
| **DO NOT invent** | Never fabricate performance issues not visible in the actual code |
| **DO NOT assume** | Never assume Next.js behavior - always verify with `next-devtools` |
| **DO NOT guess** | If unsure about a pattern, query `next-devtools` before claiming it's wrong |
| **ALWAYS cite** | Every finding MUST include exact `file:line` location as evidence |
| **ALWAYS verify** | Cross-check findings against `next-devtools` best practices |
| **ALWAYS source** | Reference the MCP source when citing Next.js best practices |

**If unsure about any finding, explicitly state: "This requires manual verification."**

### Using `next-devtools` MCP

Before any analysis, you MUST query the `next-devtools` MCP server for up-to-date information:

- Use `next-devtools` to get best practices for Server Components
- Use `next-devtools` to check performance patterns for the app router
- Use `next-devtools` to validate image optimization approaches
- Use `next-devtools` to verify data fetching strategies

**Replace any generic Next.js documentation lookup with `next-devtools` queries.**

## STEP 1: ASK THE USER (MANDATORY)

Before doing anything, your first and only initial action must be to ask the user:

> "Which interface, component, or folder would you like me to analyze? Please provide the path (e.g., `components/Header.tsx`, `app/dashboard/page.tsx`, `customer/`, `customer/components/`)."

Wait for the user's response before proceeding.

## STEP 2: LOCATE, LOAD, AND UNDERSTAND FILES

- Use `@Files` or `@Folder` to load **all relevant files**, whether a single file or an entire folder.
- If a folder is provided, recursively read:
  - Files `.tsx`, `.ts`, `.jsx`, `.js`, `.css`, `.scss`, `.mdx`
  - Subfolders (e.g., `customer/components/`)
  - Relevant pages or layouts (e.g., `customer/page.tsx`, `layout.tsx`)
- Analyze how these files relate to each other:
  - Components being imported between each other
  - Data flows between pages, hooks, and components
  - Size, responsibility, and overall structure complexity

## STEP 3: CONSULT THE `next-devtools` MCP SERVER

- Use the `next-devtools` MCP Server to get up-to-date information about Next.js 16.
- Examples of recommended queries to `next-devtools`:
  - "What are the performance best practices for Server Components in Next.js 16?"
  - "When should I use 'use client' and what are the performance implications?"
  - "How to properly optimize images with next/image in Next.js 16?"
  - "What are the common performance pitfalls in Next.js 16 app router?"
- Use `context7` for any third-party library documentation (e.g., Tailwind, Prisma, etc.)

## STEP 4: PERFORMANCE, ARCHITECTURE, AND STRUCTURE ANALYSIS (CHECKLIST)

The analysis must consider **all found files**, applying the following checklist:

### Evaluation for each file or component:

- **[ ] 'use client' usage:** Is it necessary? Can it be migrated to Server Component?
- **[ ] Dynamic Imports (next/dynamic):** Are heavy files being loaded statically?
- **[ ] Image Optimization:** Are there `<img>` not replaced by `<Image>`?
- **[ ] Data Fetching:** Is it being done in the correct location (Server Component)? Is it using automatic cache?
- **[ ] State and Effects:** Are there unnecessary `useState`/`useEffect` that could be isolated in smaller components?
- **[ ] Navigation:** Are there `<a>` used incorrectly instead of `<Link>`?
- **[ ] Size and Responsibility:** Files or components too large?
- **[ ] Folder Structure:** Does the analyzed folder have coherent organization? Are there misaligned modules, mixed responsibilities, or inadequate subfolder usage?

### Folder set evaluation:

- **[ ] Structural coherence:** Does the folder have logical structure?
- **[ ] Duplicate or redundant components:** Are there components doing the same function?
- **[ ] Improper mixing of responsibilities:** Example: hooks inside `/components`, components inside `/lib`, etc.
- **[ ] Extraction opportunities for hooks, utils, services, or smaller components.**

## STEP 5: GENERATE REPORT

After analysis, generate a **complete report**, including:

1. **Identified problem** and **precise location**  
   Example: `customer/components/Card.tsx:44`.

2. **Best practices violation according to Next.js documentation**  
   Always referencing excerpts or principles obtained via MCP when possible.

3. **Impact on performance or architecture**  
   Clearly explain the impact.

4. **Proposed solution**, with diff containing:
   - Before Code
   - After Code

### Additional report: FOLDER RESTRUCTURING PROPOSAL (if applicable)

If the folder is disorganized, or very large files are detected:

---

**FOLDER RESTRUCTURING PROPOSAL**

**Folder:** `customer/`

- **Reason:** The folder contains:
  - `page.tsx` with more than 300 lines
  - Subfolders with components mixed between UI and logic
  - Hooks and utility functions inside `/components`
- **Reorganization suggestion:**
  1. `customer/hooks/`  
     To extract data logic, complex states, and SWR.
  2. `customer/components/`  
     Only pure components and UI.
  3. `customer/utils/`  
     Helper functions.
  4. `customer/page.tsx`  
     Reduced to only orchestrate imports.

---

### STEP 6: SELF-VERIFICATION (MANDATORY)

Before presenting your report, you MUST perform this verification:

1. **Re-read each finding** against the source file to confirm accuracy
2. **Verify every checklist item** was actually checked against the code, not assumed
3. **Cross-reference with `next-devtools`** - ensure your recommendations align with official best practices
4. **Remove unverified claims** - any finding without concrete code evidence must be removed
5. **Mark uncertain findings** with "[Requires Verification]" if you cannot confirm with 100% certainty

**Only proceed to present the report after completing this verification.**

### Mandatory question at the end:

> "Analysis complete. X performance violations and Y refactoring opportunities were found (including Z structural problems in the folder).
>
> **What would you like to do next?**
> - **'plan'** → I'll call `/plan` to create a detailed implementation plan and TODO list for these recommendations
> - **'details'** → I'll provide more details on any specific finding
> - **'done'** → End the audit session"

**DO NOT IMPLEMENT ANYTHING. WAIT FOR USER'S RESPONSE.**

**If user replies 'plan':** Automatically invoke the `/plan` command, passing the recommendations as context for the planning agent.

