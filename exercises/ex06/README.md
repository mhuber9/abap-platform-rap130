[Home - Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code](../../README.md)

# Exercise 6: Debug ABAP Code in Visual Studio Code _(Optional)_

## Introduction

You will debug the executable Travel application from [Exercise 4](../ex04/README.md). Follow the call from the console class into `save_travel` and `validate_customer`, and inspect why one customer is accepted and another is rejected.

### Exercises

- [6.1 - Set Breakpoints in the Service and Helper](#exercise-61-set-breakpoints-in-the-service-and-helper)
- [6.2 - Trigger and Attach the Debugger](#exercise-62-trigger-and-attach-the-debugger)
- [6.3 - Inspect Variables and Step Through Code](#exercise-63-inspect-variables-and-step-through-code)
- [6.4 - Use the Watch View and Call Stack](#exercise-64-use-the-watch-view-and-call-stack)
- [Summary](#summary)

> Replace `####` with your four-digit group ID. Complete Exercise 4 first and activate all source changes before debugging.

---

## Exercise 6.1: Set Breakpoints in the Service and Helper
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Open `YCL_TRAVEL_SERVICE_####` using **ABAP: Open Object**.
2. Navigate to `save_travel` using the Outline or **Go to Symbol in Editor**.
3. Click the editor gutter beside the helper call to set a breakpoint.
4. Set another breakpoint on `MODIFY ytravel#### FROM @is_travel`.
5. Open `YCL_TRAVEL_HELPER_####` and set a breakpoint on the `SELECT SINGLE` statement.
6. Open **Run & Debug** from the Activity Bar, or press **Ctrl+Shift+D** (macOS: **Cmd+Shift+D**), and confirm all three entries appear in the Breakpoints panel.

> The first breakpoint identifies each save attempt; the SQL-write breakpoint shows whether that attempt reaches persistence.

</details>

---

## Exercise 6.2: Trigger and Attach the Debugger
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Open `YCL_TRAVEL_APP_####`. Ensure its fixed travel/customer constants still match your sample data and that the invalid customer is absent.
2. Run **ABAP: Run ABAP Application (Console)** from the Command Palette with the same destination and user used to set the breakpoints.
3. If ADT requests permission to attach to the ABAP debugging session, accept it. The editor should pause at the service's helper call for the valid save.
4. Now that execution is paused, open **Run & Debug** if needed. The debugger panels and debug toolbar are now fully visible. Familiarize yourself with the panels:

   | Panel | Purpose |
   |-------|---------|
   | Variables | Values in the selected stack frame |
   | Watch | Expressions you want to inspect repeatedly |
   | Call Stack | Method calls leading to the current statement |
   | Breakpoints | Active and disabled breakpoints |

5. Locate **Continue**, **Step Over**, **Step Into**, **Step Out**, and **Stop** in the debug toolbar. Standard VS Code shortcuts are F5, F10, F11, Shift+F11, and Shift+F5 respectively; use toolbar controls if your keybindings differ. You will use these controls in the next section.
6. Confirm the current source statement and the Variables panel are visible before stepping.

See [VS Code debugging documentation](https://code.visualstudio.com/docs/debugtest/debugging) for the debugger interface and [SAP's ADT tutorial](https://developers.sap.com/tutorials/abap-environment-adt-coretools-vscode) for ABAP tooling.

If execution finishes without stopping, verify that breakpoints are enabled and bound, source is active, and the selected class reaches `save_travel`. Recheck the destination and user. If your backend or ADT version cannot attach, record the version and ask the instructor to check debugger support and authorizations; the console and unit-test exercises remain usable.

</details>

---

## Exercise 6.3: Inspect Variables and Step Through Code
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. At the first service breakpoint, expand `is_travel`. Check its travel and customer IDs against the valid constants in the executable class.
2. **Step Into** `validate_customer`. Inspect `iv_customer_id`.
3. **Step Over** the lookup and inspect `rv_exists`. It should be true for the existing customer.
4. **Step Out** to the service and continue to the `MODIFY` breakpoint. This valid call must reach the write.
5. Continue until the next helper call. This is the invalid-customer attempt. Verify the customer ID differs while the travel ID stays the same.
6. Step through the lookup. `rv_exists` should be false. Follow the error-message assignment and early return.
7. Confirm the invalid attempt never reaches the `MODIFY` breakpoint. In the caller, inspect the false success result and the unchanged-row comparison before rollback.

> Do not edit customer values in the debugger: use the fixed examples so the console results remain reproducible.

</details>

---

## Exercise 6.4: Use the Watch View and Call Stack
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. While paused in the service, add `is_travel-customer_id` and `rs_result-success` to **Watch**.
2. While paused in the helper, inspect `iv_customer_id` and `rv_exists`. Expressions from another frame may be unavailable; select the appropriate frame rather than treating this as a program error.
3. Inspect the **Call Stack**. Your application frames should lead through:

   ```text
   YCL_TRAVEL_APP_####     IF_OO_ADT_CLASSRUN~MAIN
     YCL_TRAVEL_SERVICE_####  SAVE_TRAVEL
       YCL_TRAVEL_HELPER_####   VALIDATE_CUSTOMER
   ```

4. Select the caller frame to see the original travel data, then return to the helper frame.
5. Continue normally to completion so the caller reaches its documented commit/rollback boundaries. Inspect the final console messages.
6. Disable or remove your breakpoints when finished.

</details>

---

## Summary
[^Top of page](#)

You triggered the ABAP debugger from a console application, followed method calls, inspected variables and watches, and verified the rejected path returns before persistence.

Continue with **[Exercise 7: Create a Custom Agent](../ex07/README.md)** or return to **[Tutorial Home](../../README.md)**.
