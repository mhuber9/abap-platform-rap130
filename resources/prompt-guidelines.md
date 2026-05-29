# Prompt Guidelines for RAP130 💎

This file provides guidelines and example prompts for using a **coding agent with ADT MCP tools** (examples shown with GitHub Copilot) effectively during the RAP130 workshop.

---

## General principles

- Always specify the **object names** and **suffix `###`** explicitly in your prompt
- Specify the **target package** when creating or generating objects
- Allow MCP tool calls when Copilot requests them — review the parameters before approving
- If a generation produces unexpected results, refine the prompt and try again

---

## Example prompts by exercise


### Exercise 2 — List generators

```
List all available ABAP RAP generators using the MCP tool.
```

### Exercise 2 — Run the RAP generator

```
Generate a transactional SAP Fiori app for travel management with DRAFT using the ABAP RAP generator.

Use the following specification:
- Package: ZRAP130_AI_###
- Entity 1: Travel, based on the structure /DMO/TRAVEL_DATA
- Entity 2: Booking, based on /DMO/BOOKING_DATA (child of Travel)
- All generated object names should end with the suffix "###"
```

### Exercise 2 — Activate all objects

```
Activate all objects in package ZRAP130_AI_### using the MCP tool.
```


### Exercise 4 — Implement validation

```
Add the validation validateCustomer. In its implementation, the method validate_customer from the ABAP ZCL_TRAVEL_HELPER_### should be used. Ask to review changes before proceeding with the activation
```

### Exercise 5 — Generate unit tests

```
Generate ABAP unit tests for the local test class LTCL_TRAVEL_HELPER_### in the class ZCL_TRAVEL_HELPER_###.
Create test methods for validate_customer (valid and invalid cases) and get_booking_status (Booked/New/Cancelled).
```

### Exercise 5 — Run unit tests via MCP

```
Run ABAP unit tests for class ZCL_TRAVEL_HELPER_### using the MCP tool.
```

---

## Tips for better MCP results

| Tip | Why it helps |
|-----|-------------|
| Be explicit about object names | Avoids ambiguity when multiple objects match |
| Specify the suffix `###` | Ensures generated artifacts don't collide with other participants |
| Say "use the MCP tool" | Signals to your coding agent to prefer tool calls over code suggestions |
| Allow all tool calls in sequence | Some operations (e.g., generate) require multiple tool calls |
| Refresh the Visual Studio Code Explorer after generation | Press **F5** to see newly created objects |
