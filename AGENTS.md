# Repository Guidelines

## Project Structure

This repository contains a bilingual ABAP workshop. `README.md` and `README.de.md` are the English and German entry points. Exercises live in `exercises/ex0/` and `exercises/ex01/` through `exercises/ex07/`; exercises 6 and 7 are optional. Each directory contains paired Markdown files and may contain shared `images/`. Reusable prompts live in `resources/prompt-guidelines*.md`. ABAP source and test examples are embedded in Markdown; participants create and execute objects in their connected SAP system.

## Development and Validation Commands

No local build or Markdown linter is configured.

- `git diff --check`: detect whitespace errors before committing.
- `git diff --stat`: review the scope of changes.
- `rg -n 'YCL_TRAVEL|YTRAVEL|YBOOKING' exercises resources`: check naming consistency.
- In VS Code, use `ABAP: Activate`, `ABAP: Run ABAP Application (Console)`, and `ABAP: Run ABAP Unit Tests` for backend validation.

Preview edited Markdown and verify relative links, heading anchors, language switches, images, code fences, and `<details>` blocks.

## Style, Naming, and Translation

Update English and German counterparts together. Keep German navigation within `.de.md` files and preserve the English/Deutsch switches. Translate prose and prompts; keep VS Code commands, UI labels, ABAP code, identifiers, and expected console messages in English.

Preserve exercise numbering, numbered steps, tables, and expandable sections. Use spaces rather than tabs and retain surrounding indentation. Use fenced `abap` blocks for ABAP examples.

Use `$TMP`, `Y`-prefixed objects, and exactly four digits for `####`, including leading zeros: `YTRAVEL0123`, `YCL_TRAVEL_APP_0123`. Application exercises use `abap-developer`. Rely on its General and Testing instructions; do not repeat virtual-workspace, editor, MCP, cloud-syntax, or routine testing rules in prompts. Keep prompts focused on application requirements and specific test cases. Preserve the ordinary ABAP class/SQL console architecture and caller-owned commit/rollback boundaries. Keep `/DMO/` reference data read-only.

## Testing Guidelines

Place ABAP Unit tests in the dedicated testclass include and run them after source or test changes. Use `CL_OSQL_TEST_ENVIRONMENT` for isolated database tests. Follow descriptive names such as `customer_exists`, `customer_not_exists`, and `initial_customer`. Cover valid, missing, and initial customers, plus rejection before persistence. Compare stored rows before rollback or cleanup. No numeric coverage threshold is configured. Report backend checks that were not executed.

## Commits and Pull Requests

Follow the existing short, imperative commit style, for example `Clarify debugger setup instructions`. PRs should describe the affected exercises, behavior changes, bilingual updates, and validation performed; link relevant issues. Include screenshots when changing illustrated UI steps. Follow the DCO process described in the README and preserve license notices.
