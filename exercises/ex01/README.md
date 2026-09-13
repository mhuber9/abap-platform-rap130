**English** | [Deutsch](README.de.md)

[Home - Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code](../../README.md)

# Exercise 1: Enable the ADT MCP Server 💎

## Introduction

In the previous exercise, you installed Visual Studio Code and the ADT for Visual Studio Code extension, and connected to your ABAP system (_see [Getting Started](../ex0/README.md)_).

In this exercise, you will enable the **ADT MCP Server** that is built into the ADT for Visual Studio Code extension, and verify that the MCP tools are available.

The ADT MCP Server exposes ABAP development capabilities as **Model Context Protocol (MCP) tools** — allowing you to create ABAP objects, activate objects, and run unit tests through natural language prompts. For more information, see [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US)

### Exercises

- [1.1 - Enable the ADT MCP Server in Visual Studio Code Settings](#exercise-11-enable-the-adt-mcp-server-in-visual-studio-code-settings)
- [1.2 - Verify the MCP Server is Running](#exercise-12-verify-the-mcp-server-is-running)
- [1.3 - Verify the ADT MCP Tools in your coding agent](#exercise-13-verify-the-adt-mcp-tools-in-your-coding-agent)
- [Summary & Next Exercise](#summary--next-exercise)

> ℹ️ **Reminder**: Don't forget to replace all occurrences of the placeholder **`####`** with your four-digit group ID in the exercise steps below.

---

## About the ADT MCP Server 💎

The **ADT MCP Server** is a local HTTP server that runs inside the ADT for Visual Studio Code extension. It implements the **Model Context Protocol (MCP)**, an open standard that allows AI assistants (like GitHub Copilot) to call tools in a structured, authenticated way.

When enabled, the MCP server exposes a set of ABAP development tools to any MCP-compatible AI client. In this workshop, the exercises use **GitHub Copilot** as the client. Any coding agent that supports Visual Studio Code's virtual workspace filesystem is compatible — GitHub Copilot is confirmed; others are also supported.

**Tools used in this workshop (confirm availability in your installed version):**

| Tool | Description |
|------|-------------|
| `abap_creation-create_object` | Creates ABAP development objects |
| `abap_activate-objects` | Activates ABAP objects in the backend system |
| `abap_run_unit_tests` | Runs ABAP unit tests |

Copilot edits ABAP source through the ADT virtual workspace and uses the available MCP tools for backend operations. Object creation and source editing are separate steps; there is no application generator in this exercise.

See [ADT MCP Tools](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/mcp-tools?locale=en-US) for the complete list of tools available.

> ⚠ **Warning regarding AI outputs** ⚠
> The ADT MCP Server is an **experimental feature** that may change at any time without notice. It is not intended for productive use. Please back up your data before using it.

> **Further reading**: [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

---

## Exercise 1.1: Enable the ADT MCP Server in Visual Studio Code Settings
[^Top of page](#)

> Enable the built-in ADT MCP Server in the Visual Studio Code extension settings.

<details>
  <summary>🔵 Click to expand!</summary>

1. Open **Visual Studio Code Settings**:
   - Open the **Command Palette** with **`Ctrl+Shift+P`** (macOS: **`Cmd+Shift+P`**)
   - Type `Preferences: Open Settings (UI)` and select **Preferences: Open Settings...**.

2. In the search bar, type:
   ```
   adt mcp
   ```

3. Locate the setting **"Adt: Enable MCP Server"** (or similar) and **enable it** by checking the checkbox.

   > ℹ️ **Hint**: You can also switch to JSON mode (click the `{}` icon top-right in Settings) and add:
   > ```json
   >  "adt.mcpServer.enabled": true
   > ```

4. Optionally, you can configure the **MCP server port** (default: `2236`). Only change this if port 2236 is occupied on your machine:
   - Search for `ADT MCP port` in Settings
   - Set it to any available port between `0` and `65535`

5. **Reload Visual Studio Code** if prompted, or close and reopen the application.

   ![ADT MCP Server setting enabled](images/ex1_mcp_setting.png)

> ⚠️ **Important**: Make sure your system destination is still added to the workspace (from Exercise 0.4). The MCP server only starts once a destination is active in the workspace.

</details>

---

## Exercise 1.2: Verify the MCP Server is Running
[^Top of page](#)

> Confirm that the ADT MCP Server started successfully.

<details>
  <summary>🔵 Click to expand!</summary>

After enabling the setting and having a destination in the workspace, the server should start automatically.

### Method 1: Check for the startup notification

1. Look at the **bottom-right corner** of Visual Studio Code for a notification:
   ```
   ADT MCP Server running on port 2236
   ```

   ![ADT MCP Server running](images/ex1_mcp_running.png)

   > ℹ️ If you don't see the notification, proceed to Method 2.

### Method 2: Check via MCP Server list

1. Open the **Command Palette** (**`Ctrl+Shift+P`**).

2. Type:
   ```
   >MCP: List Servers
   ```

3. Select **"MCP: List Servers"** from the list.

4. You should see an entry for **ADT MCP Server** in the quick pick list.

5. Select it and click **Start Server** if it is not already running.

   ![MCP Server listed and running](images/ex1_mcp_list_servers.png)

### Troubleshooting

If the ADT MCP Server does not appear or fails to start:

1. **Disable and re-enable** the "Adt: Enable MCP Server" setting.
2. **Remove** the destination folder from the workspace and **add it back** (Command Palette → `ABAP: Add Destination as Folder to the Workspace...`).
3. **Restart** Visual Studio Code.

</details>

---

## Exercise 1.3: Verify the ADT MCP Tools in your coding agent
[^Top of page](#)

> Select **`abap-developer`** in GitHub Copilot Chat and confirm that the ADT MCP tools are loaded and available.

<details>
  <summary>🔵 Click to expand!</summary>

1. Open **GitHub Copilot Chat** in Visual Studio Code:
   - Click the **Copilot icon** in the Activity Bar (left), or
   - Press **`Ctrl+Shift+I`** (macOS: **`Cmd+Shift+I`**)

2. Select **`abap-developer`** in the chat agent dropdown. Configure its package as `$TMP`, object prefix as `Y`, and MCP server as your connected ADT MCP server. Its General and Testing instructions already define cloud-compliant ABAP, directory-first searches, editing through VS Code, test-include placement, and test execution after source or test changes. The exercise prompts focus on application requirements rather than repeating those rules.

   > ℹ️ Use **`abap-developer`** throughout the application exercises. If it is missing from the dropdown, ask the instructor to provide the workshop agent configuration before continuing.

3. Click the **"Configure Tools"** (tools wrench icon) button in the Copilot chat input bar.

4. A quick pick list appears showing available tool providers. You should see:
   - **ADT MCP Server** with a list of tools below it (e.g., `abap_creation-create_object`, `abap_activate-objects`, etc.)


   > ✅ If you see the ADT MCP Server and its tools listed, your setup is complete!

   ![ADT MCP tools visible in Copilot Configure Tools](images/ex1_mcp_tools_visible.png)

5. Make sure the ADT MCP Server tools are **checked/enabled** in the list.

6. **Tool discovery** — enter this prompt in Copilot Chat:
   ```text
   Inspect the ADT MCP tools available in this session. Identify the tools for
   creating ABAP classes and database tables, activating objects, and running
   ABAP Unit tests. Do not create, change, or activate any objects yet.
   ```

7. Compare the answer with the enabled tool list. A description alone does not prove a backend connection. Ask Copilot to read the existing `/DMO/TRAVEL_DATA` and `/DMO/BOOKING_DATA` structures through the connected ADT workspace and summarize their keys and fields without editing them.

8. Review any requested read operation and its system destination. Confirm that actual object contents are returned. If access fails, reconnect the destination and check that the Flight Reference Scenario is installed before continuing.

   > ✅ Success: Copilot can access existing ABAP source, and the creation, activation, and test tools are available. If your version lacks creation support for a required object type, use **ABAP: Create New ABAP Object** for that object in Exercise 2, then let Copilot edit its source.

</details>

---

## Summary & Next Exercise
[^Top of page](#)

Now that you've:
- Enabled the ADT MCP Server in Visual Studio Code extension settings
- Verified the server is running (via notification or `MCP: List Servers`)
- Confirmed that the ADT MCP tools are visible and callable from **`abap-developer`** in GitHub Copilot Chat

you can continue with the next exercise — **[Exercise 2: Generate the ABAP Travel Application](../ex02/README.md)**

---
