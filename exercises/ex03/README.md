[Home - Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code](../../README.md)

# Exercise 3: Run the Travel Application

## Introduction

In [Exercise 2](../ex02/README.md), you created the Travel tables and classes. Now you will load a small sample, display travels with their bookings in the ABAP Console, and create the customer-validation helper used in the next exercise.

### Exercises

- [3.1 - Load Travel and Booking Demo Data](#exercise-31-load-travel-and-booking-demo-data)
- [3.2 - Run and Inspect Console Output](#exercise-32-run-and-inspect-console-output)
- [3.3 - Create the Customer-Validation Helper](#exercise-33-create-the-customer-validation-helper)
- [Summary & Next Exercise](#summary--next-exercise)

> Replace `###` with your group ID. Use only your participant tables for writes.

---

## Exercise 3.1: Load Travel and Booking Demo Data
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Open `ZCL_TRAVEL_SERVICE_###` and ask Copilot in Agent mode:

   ```text
   Implement load_demo_data in ZCL_TRAVEL_SERVICE_###.
   If either ZTRAVEL### or ZBOOKING### already contains data in the current client,
   return 'Demo data already present; loading skipped' without changing anything.
   Otherwise read up to five /DMO/TRAVEL rows ordered by travel_id that have at least
   one /DMO/BOOKING row and an existing non-initial customer in /DMO/CUSTOMER.
   Read only the bookings belonging to those selected travels.
   If no matching source travels exist, return 'No demo source data found' and write nothing.
   Map the source rows to our tables using the definitions inspected in Exercise 2.
   Explicitly handle differing field names, statuses, and audit fields; do not assume
   CORRESPONDING alone maps them all. Populate the current client as needed.
   Insert travels before bookings. Do not commit or roll back in this method.
   Let database exceptions reach the caller so it can roll back the whole load.
   Return 'Demo data loaded' on success. Never delete or update /DMO/ data.

   Update ZCL_TRAVEL_APP_### main to call load_demo_data once, COMMIT WORK on
   normal return, and catch cx_sy_open_sql_db to ROLLBACK WORK, print the error,
   and return on failure. Then print the loader message, travel count, and each
   travel followed by its bookings using read_travels, read_bookings and out->write.
   If no travels exist, print 'No travel data available' and return.
   Keep all SELECT/INSERT statements in the service. Show changes for review before activation.
   Do not execute the class yet.
   ```

2. Review the mapping. The source Travel table can use `status` while the reference structure uses `overall_status`; have Copilot explain the mapping it found in your system. Check that booking selection cannot accidentally read the entire source table when no travels were selected.

3. Check that a second run will skip loading, that all writes use your suffix, and that the caller rolls back a failed load before returning.

4. Confirm activation of the service and executable class.

</details>

---

## Exercise 3.2: Run and Inspect Console Output
[^Top of page](#)

<details>
  <summary>🔵 Click to expand!</summary>

1. Open `ZCL_TRAVEL_APP_###` in the editor.

2. Open the Command Palette and select **ABAP: Run ABAP Application (Console)**. Select the class if prompted. Use the console execution command supplied by your ADT version; do not select ABAP Unit test execution.

   See SAP's [console execution documentation](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/testing-and-quality-checking) for this command.

3. Inspect the **Output** panel containing the ABAP console output. The layout and IDs depend on your source data. The output should contain these sections:

   ```text
   Demo data loaded
   Travel count: <1 to 5>
   Travel: <travel_id>, Customer: <customer_id>, ...
   Bookings for travel <travel_id>: <booking rows>
   ...
   ```

4. Confirm each booking's `travel_id` matches the travel printed above it. Note one travel ID and its valid customer ID for Exercise 4.

5. Run the class again. Expect `Demo data already present; loading skipped`, with the same travel and booking counts. Loading must not duplicate records or reset later changes.

6. If the output says `No demo source data found`, ask the instructor to provision Flight Reference Scenario data. If loading is skipped but no travels appear, inspect your participant tables for partial data from an earlier attempt; resolve that with the instructor before continuing. Do not clear shared source tables.

> ✅ Success: console output shows Travel and Booking data, and a repeated run preserves it.

</details>

---

## Exercise 3.3: Create the Customer-Validation Helper
[^Top of page](#)

> This helper checks whether a customer exists. Demo loading belongs to the service created above.

<details>
  <summary>🔵 Click to expand!</summary>

1. In the Command Palette, choose **ABAP: Create New ABAP Object**, then **Class**.
2. Enter package `ZRAP130_AI_###`, name `ZCL_TRAVEL_HELPER_###`, and description `Travel customer validation ###`. Leave superclass and interface empty.
3. Replace the class source with the following, substituting your group ID:

   ```abap
   CLASS zcl_travel_helper_### DEFINITION
     PUBLIC FINAL CREATE PUBLIC.
     PUBLIC SECTION.
       METHODS validate_customer
         IMPORTING iv_customer_id TYPE /dmo/customer_id
         RETURNING VALUE(rv_exists) TYPE abap_bool.
   ENDCLASS.

   CLASS zcl_travel_helper_### IMPLEMENTATION.
     METHOD validate_customer.
       rv_exists = abap_false.
       IF iv_customer_id IS INITIAL.
         RETURN.
       ENDIF.
       SELECT SINGLE FROM /dmo/customer
         FIELDS @abap_true
         WHERE customer_id = @iv_customer_id
         INTO @rv_exists.
     ENDMETHOD.
   ENDCLASS.
   ```

4. Review the early return for an initial customer ID and the read-only lookup. Save and activate the helper.

</details>

---

## Summary & Next Exercise
[^Top of page](#)

You loaded participant demo tables, ran the executable class, inspected Travel and Booking console output, and created a reusable customer-validation helper.

Continue with **[Exercise 4: Add a Validation](../ex04/README.md)**.
