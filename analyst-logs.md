# Log Auditor (Quality and Relevance)

## Objective

To critically analyze the quality and relevance of all logs in the project, regardless of the method used (`console.log`, `this.logger`, etc.). The goal is to generate an audit report with recommendations to ensure logs are useful, secure, and appropriate for a production environment.

## ANALYSIS GUIDELINES (DO NOT FOCUS ON THE METHOD)

Look for ANY type of logging call (`console.*`, `this.logger.*`, `logger.*`, etc.) and classify it based on the following rules:

### Logs that are USUALLY APPROPRIATE:

- **Error Logs in `catch` Blocks:** Essential for exception tracking.
- **Security Logs:** Records of security events (login, logout, authentication failures, permission changes).
- **Critical Business Logs:** Business events that cannot fail (e.g., "Payment confirmed", "Order dispatched").

### Logs that are USUALLY INAPPROPRIATE or HARMFUL:

- **Debug Logs:** Any log with the purpose of debugging (e.g., `this.logger.debug('Component mounted')`, `console.log(data)`).
- **Unnecessary Verbose Logs:** Information about the application lifecycle that doesn't help in production (e.g., "API started", "Connected to database").
- **Logs with Sensitive Data:** Logs that display passwords, tokens, PII (Personally Identifiable Information), or internal company data.
- **Logs that Expose Internal Structure:** Logs that print raw objects or errors that could reveal vulnerabilities or implementation details.

## STEP 1: ASK THE USER (MANDATORY)

Your first and only initial action must be to ask the user:

> "Which file or folder would you like me to audit regarding log quality? Please provide the path (e.g., `src/auth/`, `services/payment.service.ts`, or `app/dashboard/`)."

Wait for the user's response before proceeding.

## STEP 2: LOAD AND ANALYZE THE CODE

- Use the `@Files` or `@Folders` symbol to load the content of the file or all files in the specified folder.
- Look for all logging calls, not limiting yourself to `console`.

## STEP 3: GENERATE AUDIT REPORT

For each log found, present a detailed report.

### Example Report:

---

**File:** `src/services/payment.service.ts`

1.  **Line 25:** `this.logger.log(`Processing payment for user ${userId}`);`

    - **Method:** `this.logger.log`
    - **Classification:** **APPROPRIATE**
    - **Justification:** It's a critical business log that tracks an important action.

2.  **Line 48:** `this.logger.debug('Webhook request received');`

    - **Method:** `this.logger.debug`
    - **Classification:** **INAPPROPRIATE (Recommendation: Remove)**
    - **Justification:** Verbose and unnecessary debug log in production. It pollutes the logs and can impact performance.

3.  **Line 75:** `console.error(error);`

    - **Method:** `console.error` (outside a visible `catch` block)
    - **Classification:** **INAPPROPRIATE (Recommendation: Refactor)**
    - **Justification:** Error log without context. It should be inside a `try/catch` and use `this.logger.error` with a clear message.

4.  **Line 102:** `this.logger.info(`Access token: ${userToken}`);`
    - **Method:** `this.logger.info`
    - **Classification:** **CRITICAL (Recommendation: Remove Immediately)**
    - **Justification:** **EXPOSURE OF SENSITIVE DATA (Token).** This is a serious security risk.

---

### FINAL MANDATORY QUESTION

After presenting the complete report, ask:

> "Audit complete. I found X appropriate logs, Y inappropriate logs, and Z critical ones.
>
> **What would you like to do next?**
> - **'plan'** → I'll call `/plan` to create an implementation plan for cleaning up logs
> - **'details'** → I'll provide more details on specific log findings
> - **'done'** → End the audit session"

**DO NOTHING BEYOND THIS. WAIT FOR THE USER'S RESPONSE.**

**If user replies 'plan':** Automatically invoke the `/plan` command, passing the log cleanup recommendations as context.

