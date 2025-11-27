# Fix Imports to Path Aliases (TypeScript)

## Objective

Find imports that use complex relative paths (e.g., `../../../utils/function.tsx`) and replace them with alias paths configured in the project's `tsconfig.json`, resulting in cleaner and more maintainable imports (e.g., `@/utils/function.tsx`).

## STEP 1: ASK THE USER (MANDATORY)

Your first and only initial action must be to ask the user:

> "Which file or folder would you like me to review to fix imports? Please provide the path (e.g., `components/`, `src/services/user.service.ts`, or `app/dashboard/`)."

Wait for the user's response before proceeding.

## STEP 2: READ AND UNDERSTAND `tsconfig.json`

- **Before analyzing any file, use `@Files` to load the `tsconfig.json` at project root.**
- Analyze the `compilerOptions.paths` section to identify all available path aliases (e.g., `@/*`, `@components/*`, `@lib/*`).
- Create a mental map of these aliases to use in replacements.

## STEP 3: ANALYZE FILES AND IDENTIFY PROBLEMATIC IMPORTS

- Use the `@Files` or `@Folders` symbol to load the content of the file or all files in the specified folder.
- Look for all `import` declarations that use relative paths with multiple levels (e.g., `../..`, `../../..`).
- **Ignore simple imports** like `./Component` or `../types`, as they are generally acceptable.

## STEP 4: MAP TO CORRECT ALIAS AND GENERATE REPORT

For each problematic import found:

1. **Calculate the absolute path** from project root.
2. **Check if there's an alias** in `tsconfig.json` that matches this absolute path.
3. **If there's a matching alias**, propose the replacement.

### Report Example:

---

**File:** `src/components/forms/UserForm.tsx`

1. **Line 5:** `import { validateUser } from '../../../utils/validation';`

   - **Action:** **REPLACE**
   - **Justification:** The relative path is long and fragile. Using the `@/` alias defined in `tsconfig.json`, the import becomes cleaner and resistant to folder structure changes.
   - **Proposal:**
     ```diff
     - import { validateUser } from '../../../utils/validation';
     + import { validateUser } from '@/utils/validation';
     ```

2. **Line 8:** `import { Button } from '../Button';`

   - **Action:** **KEEP**
   - **Justification:** Simple and local relative import. Replacement wouldn't bring significant benefits.

3. **Line 12:** `import { api } from '../../../../services/api';`
   - **Action:** **REPLACE**
   - **Justification:** Excessively complex relative path.
   - **Proposal:**
     ```diff
     - import { api } from '../../../../services/api';
     + import { api } from '@/services/api';
     ```

---

## FINAL MANDATORY QUESTION

After presenting the complete report, ask:

> "Analysis complete. I found X imports to replace with alias paths. Would you like me to apply all suggested corrections? Please reply **'yes'** to proceed or **'no'** to cancel."

**Wait for an explicit user response. Do not make any changes until you have permission.**

## STEP 5: APPLY CORRECTIONS AND VERIFY

- **Execute this step ONLY if the user replies 'yes'.**
- Apply each of the proposed replacements in the report.
- **After applying changes, run the linter and type-checker (`tsc --noEmit`) to ensure paths are correct and code compiles.**

