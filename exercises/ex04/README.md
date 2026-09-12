[Home - Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code](../../README.md)

# Exercise 4: Add a Validation

## Introduction

In [Exercise 3](../ex03/README.md), you created `YCL_TRAVEL_HELPER_####->validate_customer`. Now you will ask Copilot to call it from the service before saving a travel. The console application will demonstrate an accepted save and a rejected save.

### Exercises

- [4.1 - Define and Implement the Validation](#exercise-41-define-and-implement-the-validation)
- [4.2 - Run and Test the Enhanced Travel Application](#exercise-42-run-and-test-the-enhanced-travel-application)
- [Summary & Next Exercise](#summary--next-exercise)

> Replace `####` with your four-digit group ID. Review generated changes before activation.

---

## Exercise 4.1: Define and Implement the Validation
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Open `YCL_TRAVEL_SERVICE_####` and GitHub Copilot Chat, then select **`abap-developer`** in the agent dropdown.

2. Enter the following prompt:

   ```text
   Add customer validation to save_travel in YCL_TRAVEL_SERVICE_####.
   Keep its existing signature and ty_result (success and message).
   After checking that travel_id is not initial, call validate_customer on
   YCL_TRAVEL_HELPER_#### with is_travel-customer_id.
   If the helper returns abap_false, return success = abap_false and
   'Customer <ID> does not exist' (or 'Customer ID is required' for an initial ID).
   Return before any INSERT, UPDATE, or MODIFY. Do not write invalid data.
   If valid, save the travel to YTRAVEL#### and return the actual SQL outcome.
   Do not commit or roll back here; the executable class owns the transaction.
   Ask me to review the changes before activation.
   ```

3. Review the generated method. Its control flow should match this example:

   ```abap
   METHOD save_travel.
     rs_result-success = abap_false.
     IF is_travel-travel_id IS INITIAL.
       rs_result-message = 'Travel ID is required'.
       RETURN.
     ENDIF.

     DATA(lo_helper) = NEW ycl_travel_helper_####( ).
     IF lo_helper->validate_customer( is_travel-customer_id ) = abap_false.
       rs_result-message = COND #(
         WHEN is_travel-customer_id IS INITIAL THEN 'Customer ID is required'
         ELSE |Customer { is_travel-customer_id } does not exist| ).
       RETURN.
     ENDIF.

     MODIFY ytravel#### FROM @is_travel.
     IF sy-subrc = 0.
       rs_result-success = abap_true.
       rs_result-message = |Travel { is_travel-travel_id } saved|.
     ELSE.
       rs_result-message = |Travel { is_travel-travel_id } could not be saved|.
     ENDIF.
   ENDMETHOD.
   ```

4. Confirm that the rejected path returns before `MODIFY`, and that neither the helper nor the service commits. Database exceptions are handled by the executable caller.
5. Approve activation after reviewing the source and inspect the Problems panel.

</details>

---

## Exercise 4.2: Run and Test the Enhanced Travel Application
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Use the travel ID and existing customer ID you noted in Exercise 3. Use `999999` as the customer ID for the invalid-save example.
2. Open `YCL_TRAVEL_APP_####` and submit this prompt after replacing the angle-bracket values as well as `####`:

   ```text
   Extend YCL_TRAVEL_APP_#### after the existing demo loading and display.
   Use fixed constants for travel ID <travel ID>, valid customer ID <existing ID>,
   and invalid customer ID '999999'. Do not add interactive input.
   Read the selected travel through read_travels. If absent, print a clear message
   and return without saving. Preserve the full row as the starting value.

   First set its customer_id to the valid ID and description to 'Copilot validation demo'.
   Call save_travel. Print the returned success and message. COMMIT WORK only on
   success; otherwise ROLLBACK WORK, print the failure, and return.
   Read the persisted travel again and keep that full row as the baseline.

   Copy the baseline and change only customer_id to the invalid ID.
   Call save_travel and print its result. Read the travel immediately after the call,
   before rollback, and compare the full row to the baseline. Print whether it is unchanged.
   ROLLBACK WORK after this intentionally invalid attempt, even if the service
   unexpectedly reports success. Print an explicit failure if the result was accepted
   or the stored row changed. Re-read and display the final persisted travel.

   Catch cx_sy_open_sql_db around save operations, roll back, print the error, and return.
   Keep SQL in the service. Show the code for review before activation; do not run it yet.
   ```

3. Review the transaction boundaries and confirm activation. Run the executable class using **ABAP: Run ABAP Application (Console)**.

4. Expect the following outcomes (IDs and boolean formatting vary):

   ```text
   Demo data already present; loading skipped
   ... Travel and Booking output ...
   Valid save: success = X; Travel <ID> saved
   Invalid save: success = <false>; Customer 999999 does not exist
   Stored row unchanged before rollback: X
   Final travel: <original valid customer>, Copilot validation demo
   ```

If `999999` exists in your system, use a different customer ID for the invalid-save example and rerun.

5. Confirm that the bookings remain attached to the same travel. Run again and verify that sample loading does not reset the saved description.

> Checking the row **before rollback** proves that validation prevented the write. Checking only after rollback could hide an invalid write that was later undone.

</details>

---

## Summary & Next Exercise
[^Top of page](#)

You used Copilot to integrate a helper into a normal ABAP save method, reviewed the change, and verified accepted and rejected saves through console output.

Continue with **[Exercise 5: Generate ABAP Unit Tests](../ex05/README.md)**.
