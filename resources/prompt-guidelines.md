**English** | [Deutsch](prompt-guidelines.de.md)

# Prompt Guidelines for the ABAP Travel Workshop

Use these prompts with **`abap-developer`** selected in **GitHub Copilot Chat**, connected to the **ADT MCP tools**. Use package `$TMP` and replace `####` in class and table names with your four-digit participant suffix before sending. The linked exercises contain the complete specifications; use those for initial creation instead of asking Copilot to invent the application contract.

## General principles

- Specify exact object names, package `$TMP`, and the connected destination.
- Rely on the configured General and Testing instructions in `abap-developer`. Do not repeat cloud syntax, MCP usage, virtual-workspace search, editor operations, or routine testing rules in every prompt.
- Specify application behavior, relevant objects, and scenario-specific test cases.
- Review the source before activation and tests. Review proposed fixes rather than weakening failing assertions.
- Keep sample loading repeatable and `/DMO/` data read-only. The executable class owns transaction boundaries.
- Distinguish observed tool results from suggested code or unexecuted checks.

## Example prompts by exercise

### Exercise 1 — Discover tools and read source

```text
Identify the available ADT tools for creating classes and database tables,
activating objects, and running unit tests. Read /DMO/TRAVEL_DATA and
/DMO/BOOKING_DATA and summarize their keys.
Do not change or activate anything.
```

### Exercise 2 — Create the application

Use the full [Exercise 2 creation prompt](../exercises/ex02/README.md#exercise-22-generate-the-travel-tables-and-classes). It defines `YTRAVEL####`, `YBOOKING####`, `YCL_TRAVEL_SERVICE_####`, and `YCL_TRAVEL_APP_####`, including method signatures. The helper is introduced in Exercise 3.

After reviewing the source:

```text
Activate YTRAVEL#### and YBOOKING####, then YCL_TRAVEL_SERVICE_####, then
YCL_TRAVEL_APP_####. Report actual activation results.
```

### Exercise 3 — Load and display sample data

Use the full [demo-loading prompt](../exercises/ex03/README.md#exercise-31-load-travel-and-booking-demo-data), including source selection and transaction handling. For a read-only review:

```text
Review load_demo_data in YCL_TRAVEL_SERVICE_#### and the caller in YCL_TRAVEL_APP_####.
Explain why a repeated run skips existing data, how Booking rows are restricted to
selected travels, and how a failed load is rolled back. Do not change anything.
```

### Exercise 4 — Add customer validation

```text
In YCL_TRAVEL_SERVICE_####->save_travel, use
YCL_TRAVEL_HELPER_####->validate_customer before any database write.
Reject initial or nonexistent customers with ty_result-success = abap_false
and a meaningful message. Preserve the signature and caller-owned transactions.
Show changes for review before activation.
```

Use the full [console validation prompt](../exercises/ex04/README.md#exercise-42-run-and-test-the-enhanced-travel-application) to demonstrate valid and invalid saves and compare the stored row before rollback.

### Exercise 5 — Generate and run tests

```text
Generate isolated ABAP Unit tests for YCL_TRAVEL_HELPER_####->validate_customer
using SQL doubles for /DMO/CUSTOMER. Cover existing, missing, and initial IDs.
Show tests for review before activation and execution.
```

Use the [service test cases](../exercises/ex05/README.md#exercise-52-verify-rejection-before-persistence) to prove invalid saves do not insert or change participant rows. After reviewing both classes:

```text
Run ABAP Unit tests for YCL_TRAVEL_HELPER_#### and YCL_TRAVEL_SERVICE_####.
Report the actual results. Explain any failures and propose
corrections for review without removing the failed requirements.
```

### Exercise 6 — Prepare debugging

```text
Read the console application and identify breakpoint locations in save_travel
and validate_customer for tracing the valid and invalid customer attempts.
Explain the expected call stack. Do not change the code.
```

### Exercise 7 — Verify custom-agent context

```text
Summarize my package, object names, entry point, and transaction rules.
Inspect save_travel and explain how invalid customer IDs are rejected.
Do not change or activate anything.
```

## Tips for better results

| Tip | Why it helps |
|-----|-------------|
| Include the actual activation or test error | Grounds the correction in observed behavior |
| Preserve the method contract from Exercise 2 | Keeps prompts and later exercises compatible |
| Confirm absent demo customers in the backend | Avoids assuming a hard-coded ID is invalid |
| Use test doubles for unit-test customer IDs | Keeps tests independent of shared data |
| Check stored rows before rollback or cleanup | Detects invalid writes that cleanup could conceal |
