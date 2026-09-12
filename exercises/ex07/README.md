[Home - Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code](../../README.md)

# Exercise 7: Create a Custom Agent _(Optional)_

## Introduction

The application exercises use **`abap-developer`**. In this optional exercise, you will experiment with a separate custom agent without replacing that configuration.

A custom agent gives Copilot reusable context for the Travel application: object names, package, ABAP Cloud conventions, and the review workflow. You will create one and verify that it understands your application without repeating every detail.

### Exercises

- [7.1 - Create a Custom Agent (GitHub Copilot)](#exercise-71-create-a-custom-agent-github-copilot)
- [7.2 - Adapt the Instructions for Other Coding Agents](#exercise-72-adapt-the-instructions-for-other-coding-agents)
- [Summary](#summary)

> Replace `####` with your four-digit group ID before saving the agent instructions. Keep `$TMP` as the package.

---

## Exercise 7.1: Create a Custom Agent (GitHub Copilot)
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Open Copilot Chat and use **Configure Custom Agents**, or run **Chat: New Custom Agent** from the Command Palette.
2. Choose a workspace location and name the agent `travel-workshop`. Store it as `.github/agents/travel-workshop.agent.md` in a local workspace folder. If your workspace contains only the ADT virtual destination, choose a user-level location in the creation dialog instead.
3. Paste this definition, replacing `####` with your four-digit group ID:

   ```markdown
   ---
   name: Travel Workshop
   description: Develop the ABAP Travel console application with ADT and Copilot.
   ---

   You are an ABAP developer building a Travel and Booking console application
   in Visual Studio Code with GitHub Copilot and the ADT MCP Server.

   ## Application context
   - My four-digit participant suffix is ####. Use the existing local package $TMP.
   - Do not create a new package or transport request for workshop objects.
   - Tables: YTRAVEL#### and YBOOKING####, based on /DMO/TRAVEL_DATA and /DMO/BOOKING_DATA.
   - YCL_TRAVEL_APP_#### implements IF_OO_ADT_CLASSRUN and prints with out->write.
   - YCL_TRAVEL_SERVICE_#### owns load_demo_data, read_travels, read_bookings, and save_travel.
   - YCL_TRAVEL_HELPER_#### provides validate_customer.

   ## Working rules
   - Inspect available ADT tools and existing source before making changes.
   - Use supported MCP tools for object creation, activation, and unit-test execution;
     edit source through the ADT virtual workspace. Explain any required manual step.
   - Preserve exact object names and public method signatures from the exercises.
   - Use ordinary ABAP Cloud classes and SQL. Keep the console entry point.
   - Keep /DMO/ source data read-only. Demo loading skips if either participant table has data.
   - Validate customers before saving; reject invalid input without a database write.
   - Keep COMMIT WORK and ROLLBACK WORK in the executable caller, outside service methods.
   - Use SQL test doubles for unit tests, with isolated fixtures and no commits.
   - Present source changes for review before activation, then run approved tests.
   - Report actual activation and test outcomes; do not claim unexecuted checks passed.
   ```

4. Save the definition and select **Travel Workshop** in the agent dropdown. Confirm the ADT tools are enabled for this agent.
5. Send this read-only verification prompt:

   ```text
   Summarize my package, object names, entry point, and transaction rules.
   Inspect save_travel and explain how invalid customer IDs are rejected.
   Do not change or activate anything.
   ```

6. Confirm that Copilot names your suffixed objects and explains the early return before SQL persistence. If it does not, check which agent is selected and whether your definition was loaded.

7. When finished, select **`abap-developer`** again before returning to the application exercises.

See the official [VS Code custom-agent documentation](https://code.visualstudio.com/docs/agent-customization/custom-agents) for supported file locations and creation commands.

</details>

---

## Exercise 7.2: Adapt the Instructions for Other Coding Agents
[^Top of page](#)

> Optional alternative for participants who already use another compatible coding agent.

<details>
  <summary>🔵 Click to expand!</summary>

1. Confirm your agent supports the ADT MCP connection **and** reading/editing the ADT virtual workspace. MCP connectivity alone is insufficient for this workflow.
2. Copy the application context and working rules from section 7.1 into the instruction mechanism documented by that agent. File names and formats differ; do not assume Copilot's `.agent.md` format is portable.
3. Enable the relevant ADT tools, load the instructions, and run the same read-only verification prompt.
4. Confirm that the agent reads actual ABAP source and uses your participant suffix before asking it to make changes.

</details>

---

## Summary
[^Top of page](#)

You configured reusable instructions for the Travel application and verified that your agent can apply the package, naming, transaction, and review conventions.

**[Back to Tutorial Home](../../README.md)**
