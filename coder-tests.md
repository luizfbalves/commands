# Write Unit Tests (Vitest & TDD Mode) — With Guaranteed Passing Tests

## Objective

Analyze a specified file or folder, understand its functionality using available MCPs, and produce a comprehensive suite of **relevant and meaningful** unit tests using **Vitest**.  
All generated tests must be **valid**, **coherent**, **logically correct**, and the agent must ensure **the tests pass** before concluding the process.

The goal is not to maximize coverage but to validate real expected behaviors of the code under test.

---

## MANDATORY TOOLS & FRAMEWORK

- **Testing Framework:** Vitest (required)
- **Testing Style:** AAA pattern (Arrange, Act, Assert)
- **Required MCPs:**
  - `sequentialthinking` — to break down code behavior into testable units
  - `memory` — to store important structural decisions for consistent test generation
  - `context7` — to understand the project's architecture and existing test conventions
  - Any other available MCPs that improve understanding, including dependency inspection and documentation lookup

---

## WORKFLOW

### 1. ASK FOR TARGET (MANDATORY)

Your first and only initial action must be:

> "Which file or folder would you like me to write unit tests for? Please provide the path (e.g., `src/utils/validation.ts`, `components/Button.tsx`, or `services/api/`)."

Wait for the user's reply. Do not proceed before the response.

---

## 2. LOAD CODE AND UNDERSTAND FUNCTIONALITY

- Use `@Files` or `@Folders` to load the target file(s) and any necessary imported dependencies.
- Use `sequentialthinking` to fully decompose behavior:
  - What does the file/component do?
  - What inputs can it receive?
  - What outputs or side effects does it produce?
  - What should be mocked?
  - What constitutes incorrect or problematic input?

Document your internal reasoning (privately) to organize upcoming test cases.

---

## 3. PROPOSE A COMPLETE TEST SUITE

Produce a detailed proposal including:

### A. Suggested test file location

(e.g., `src/utils/__tests__/validation.test.ts`)

### B. Full list of test cases:

- **Happy path** cases (valid expected behavior)
- **Sad path / failure cases**
- **Edge cases** (boundary, null, undefined, empty, etc.)
- **Mocking strategy** (what to mock, how, and why)

### C. Risks and considerations:

- State any features that are **not testable** or **ambiguous**
- Explain any assumptions you need to adopt

---

## 4. PRESENT PROPOSAL AND ASK FOR PERMISSION

Ask:

> "Here is the complete proposed test suite for `[Target File/Folder]`. It covers X scenarios (success, failure, edge cases). Would you like me to generate the Vitest test files now? Reply **'yes'** to continue or **'no'** to cancel."

Wait for explicit user confirmation.

---

## 5. WRITE THE TEST FILES (ONLY IF USER REPLIES "YES")

- Create a `__tests__` directory when needed.
- Generate clean, readable Vitest test files.
- Use the AAA pattern.
- Use mocking (`vi.mock`) correctly when needed.
- Ensure correctness:
  - Use realistic expected outputs
  - Match actual code behavior
  - Avoid false positives
  - Avoid brittle assertions

### **MANDATORY: VALIDATE THAT TESTS PASS**

The agent must:

1. Re-check all test logic against the loaded source code.
2. Simulate execution mentally or through reasoning.
3. Confirm that all expectations match actual behavior.
4. Rewrite any failing or invalid tests before finalizing.

Only continue after confirming internally:

> "Yes — all tests are guaranteed to pass given the current implementation."

---

## 6. FINAL SUCCESS REPORT

After confirming tests pass:

> "Success! The complete test suite for `[Target File/Folder]` has been created. All tests are internally verified to pass with the current implementation.  
> Test files created: [list test files].  
> You can now run `npm run test` or `vitest` to execute them."

---

