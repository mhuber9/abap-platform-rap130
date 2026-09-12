[Home - Build an ABAP Travel Application with GitHub Copilot and ADT in Visual Studio Code](../../README.md)

# Exercise 2: Generate the ABAP Travel Application

## Introduction

In [Exercise 1](../ex01/README.md), you connected GitHub Copilot to the ADT MCP tools. Now you will use natural language to create ordinary ABAP repository objects for the same Travel and Booking scenario used throughout the workshop.

You will learn to supply object names and requirements, inspect source context, review generated code, and activate dependent objects. Copilot writes the application source; ADT creates and activates the repository objects.

### Exercises

- [2.1 - Open the Existing Local Package](#exercise-21-open-the-existing-local-package)
- [2.2 - Generate the Travel Tables and Classes](#exercise-22-generate-the-travel-tables-and-classes)
- [Summary & Next Exercise](#summary--next-exercise)

> Use package `$TMP` and replace `####` in object names with your four-digit group ID. Review AI-generated source before activation; correct errors with Copilot rather than accepting code merely because it was generated.

---

## Exercise 2.1: Open the Existing Local Package
[^Top of page](#)

> Use the existing local package **`$TMP`** for all objects in this workshop.

<details>
  <summary>🔵 Click to expand!</summary>

1. In **Visual Studio Code**, open the Command Palette with **Ctrl+Shift+P** (macOS: **Cmd+Shift+P**).
2. Select **ABAP: Add Package as Folder to Workspace** and choose your connected ABAP destination if prompted.
3. Enter **`$TMP`** and add it to the workspace. If it is already present, open its existing folder.
4. Use `$TMP` as the package whenever creating a table or class. Do not create a new package or transport request for these local workshop objects.
5. Check your four-digit suffix against the existing objects as described in [Getting Started](../ex0/README.md#exercise-01-define-your-group-id). Other participants may also use `$TMP`.

</details>

---

## Exercise 2.2: Generate the Travel Tables and Classes
[^Top of page](#)

> Create two database tables, a service class, and an executable class in your package.

<details>
  <summary>🔵 Click to expand!</summary>

### Step 1: Inspect the reference data

1. Open **GitHub Copilot Chat** and select **Agent mode**. You can first use **Plan mode** to review the proposed changes.

2. Enter this prompt:

   ```text
   Read /DMO/TRAVEL_DATA and /DMO/BOOKING_DATA and the source tables
   /DMO/TRAVEL and /DMO/BOOKING in the connected ABAP system.
   Explain the Travel-to-Booking keys, customer fields, currency fields, and
   any differences in field names or status values between structures and tables.
   We will build a plain ABAP Travel console application. Do not change anything yet.
   ```

3. Review the returned definitions. `travel_id` identifies a travel; a booking belongs to that travel and is identified by `travel_id` plus `booking_id` within the client.

### Step 2: Create the application

4. Replace `####` with your four-digit group ID in the following prompt. Keep `$TMP` unchanged and send it in Agent mode:

   ```text
   Create a plain ABAP Cloud Travel and Booking application in the existing local package $TMP.
   Do not create a package or transport request.
   Use ADT object-creation tools where supported and edit the source in the ADT
   virtual workspace. If an object type cannot be created with the available tools,
   tell me which object to create using ABAP: Create New ABAP Object, then continue.

   Create these objects with exactly these names:
   - YTRAVEL####: client-dependent transparent table based on /DMO/TRAVEL_DATA.
     Keys: client and travel_id. Retain the reference travel business fields,
     including customer_id, dates, amounts, currency, description, and overall_status.
   - YBOOKING####: client-dependent transparent table based on /DMO/BOOKING_DATA.
     Keys: client, travel_id, booking_id. Retain the reference booking business fields.
     Each booking must refer to a travel in YTRAVEL#### in the same client.
     Inspect the actual structures; do not duplicate keys when expanding their fields.
     Preserve amount/currency annotations in both table definitions.
   - YCL_TRAVEL_SERVICE_####: public final class with these public types and methods:
     tt_travel = standard table of YTRAVEL#### with empty key.
     tt_booking = standard table of YBOOKING#### with empty key.
     ty_result = structure with success TYPE abap_bool and message TYPE string.
     read_travels: return rt_travels TYPE tt_travel, ordered by travel_id.
     read_bookings: import iv_travel_id TYPE /dmo/travel_id;
       return rt_bookings TYPE tt_booking, ordered by booking_id for that travel.
     save_travel: import is_travel TYPE YTRAVEL####;
       return rs_result TYPE ty_result. Reject an initial travel_id; otherwise
       insert or update the supplied travel with ABAP SQL and report the outcome.
       Customer validation will be added in Exercise 4; do not add it yet.
     load_demo_data: return rv_message TYPE string. For now return
       'Demo loading will be implemented in Exercise 3' without writing data.
   - YCL_TRAVEL_APP_####: public final class implementing IF_OO_ADT_CLASSRUN.
     In main, instantiate the service, call read_travels, and use out->write to
     print 'Travel application ready' and the travel count. Do not save data yet.

   Keep SQL and business operations in the service. The executable class owns
   COMMIT WORK / ROLLBACK WORK; service methods must not commit or roll back.
   Do not create a UI, services, draft tables, or framework business objects.
   Do not modify /DMO/ objects or data. Do not run the application automatically.
   Show me the created source and ask me to review it before activation.
   ```

5. If manual creation is needed, open the Command Palette, select **ABAP: Create New ABAP Object**, choose **Database Table** or **Class**, and enter the package and exact name above. Open the created source in the workspace so Copilot can continue editing it.

### Step 3: Review the objects

6. Check the following before confirming activation:

   | Object | Review focus |
   |--------|--------------|
   | `YTRAVEL####` | Client and travel key; customer and currency fields |
   | `YBOOKING####` | Client, travel, and booking keys; matching Travel relationship |
   | `YCL_TRAVEL_SERVICE_####` | Public method contract above; SQL restricted to participant tables |
   | `YCL_TRAVEL_APP_####` | `IF_OO_ADT_CLASSRUN`, service call, and `out->write` |

7. Ask Copilot to explain the dependency order. Tables must be active before the service that uses their row types; the service must be active before the executable class.

   The executable class at this stage should be as small as this example:

   ```abap
   CLASS ycl_travel_app_#### DEFINITION
     PUBLIC FINAL CREATE PUBLIC.
     PUBLIC SECTION.
       INTERFACES if_oo_adt_classrun.
   ENDCLASS.

   CLASS ycl_travel_app_#### IMPLEMENTATION.
     METHOD if_oo_adt_classrun~main.
       DATA(lo_service) = NEW ycl_travel_service_####( ).
       DATA(lt_travels) = lo_service->read_travels( ).
       out->write( 'Travel application ready' ).
       out->write( |Travel count: { lines( lt_travels ) }| ).
     ENDMETHOD.
   ENDCLASS.
   ```

   Exercise 3 extends this entry point with loading and Travel/Booking output; Exercise 4 adds the two save attempts.

### Step 4: Activate and inspect

8. Confirm activation after review, or use **ABAP: Activate** in the Command Palette for each object in dependency order.

9. Inspect the **Problems** panel. If activation fails, provide the actual error to Copilot, review its correction, and activate again.

10. Refresh your package in the Explorer and open each of the four objects. Confirm that the public method signatures match the prompt; later exercises use these exact names.

> ✅ Success: both tables and both classes are active. The application is ready for sample loading and console execution in Exercise 3.

</details>

---

## Summary & Next Exercise
[^Top of page](#)

You opened the existing local package `$TMP`, generated Travel and Booking tables and ABAP classes with Copilot, reviewed their source, and activated the dependent objects.

Continue with **[Exercise 3: Run the Travel Application](../ex03/README.md)**.
