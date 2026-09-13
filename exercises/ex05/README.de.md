[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 5: ABAP Unit-Tests erzeugen 💎

## Einführung

In der vorherigen Übung hast du eine Backend-Validierung für Kunden-IDs ergänzt (siehe [Übung 4](../ex04/README.de.md)).

Jetzt erzeugst du isolierte ABAP Unit-Tests für Hilfsklasse und Service, prüfst sie und nutzt Copilot zur Diagnose und Korrektur von Fehlern.

### Übungen

- [5.1 – Unit-Tests erzeugen und ausführen](#übung-51-unit-tests-erzeugen-und-ausführen-)
- [5.2 – Ablehnung vor dem Datenbankschreibzugriff prüfen](#übung-52-ablehnung-vor-dem-datenbankschreibzugriff-prüfen)
- [Zusammenfassung](#zusammenfassung)

> ℹ️ **Erinnerung:** Ersetze in den folgenden Schritten den Platzhalter **`####`** durch deine vierstellige Gruppen-ID.

---

## Übung 5.1: Unit-Tests erzeugen und ausführen 💎
[↑ Zum Seitenanfang](#)

> Verwende **`abap-developer`** in GitHub Copilot Chat, um Unit-Tests für **`YCL_TRAVEL_HELPER_####`** zu erzeugen, Probleme zu beheben und die Tests auszuführen, bis alle erfolgreich sind.

> ⚠ **Hinweis zu KI-Ausgaben** ⚠
> Die hier gezeigten KI-Ausgaben können sich von deinen Ergebnissen unterscheiden. **Prüfe erzeugten Testcode immer, bevor du ihn ausführst.**

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne **GitHub Copilot Chat** und wähle **`abap-developer`** in der Agentenauswahl.
2. Gib folgenden Prompt ein:

   ```
   Erzeuge ABAP Unit-Tests für validate_customer in YCL_TRAVEL_HELPER_####.
   Verwende das ABAP SQL Test Double Framework für /DMO/CUSTOMER; die Tests dürfen
   nicht von echten Daten abhängen. Decke einen vorhandenen Kunden, einen fehlenden
   Kunden und eine initiale Kunden-ID ab. Leere die Doubles zwischen den Tests
   und zerstöre die Testumgebung in class_teardown.
   Zeige die Tests vor Aktivierung und Ausführung zur Prüfung. Führe sie nach meiner
   Prüfung aus, diagnostiziere Fehler und schlage Korrekturen vor, ohne Assertions abzuschwächen.
   ```

   > ℹ️ Ersetze `####` im Prompt vor dem Absenden durch deine vierstellige Gruppen-ID.

3. Prüfe die erzeugten Tests und bestätige Aktivierung und Ausführung. Copilot kann **`abap_activate-objects`** und **`abap_run_unit_tests`** verwenden, sofern sie in deiner installierten Werkzeugliste verfügbar sind. Prüfe bei Fehlern die Diagnose und Korrekturen und führe die Tests erneut aus.
4. Prüfe die Testergebnisse im Panel **Test Results**.
5. Optional kannst du die Unit-Tests zur Kontrolle manuell ausführen: Öffne die **Command Palette** (**`Ctrl+Shift+P`** / **`Cmd+Shift+P`**) und wähle **"ABAP: Run ABAP Unit Tests"**.
6. Prüfe, dass die lokalen Testklassen entsprechend den Anweisungen von `abap-developer` im separaten Testklassen-Include stehen und nicht im Hauptquellcode der Klasse. Dein Code sollte ungefähr so aussehen:

   > ℹ️ Ersetze **`####`** durch deine zugewiesene Gruppen-ID oder dein gewähltes Suffix.

   <details>

      ```ABAP
        
      CLASS ltc_validate_customer DEFINITION FINAL FOR TESTING
         DURATION SHORT
         RISK LEVEL HARMLESS.

         PRIVATE SECTION.
            CLASS-DATA: mo_osql_env TYPE REF TO if_osql_test_environment.
            DATA:        mo_cut      TYPE REF TO ycl_travel_helper_####.

            CLASS-METHODS:
               class_setup,
               class_teardown.
            METHODS:
               setup,
               customer_exists     FOR TESTING,
               customer_not_exists FOR TESTING,
               initial_customer    FOR TESTING.

      ENDCLASS.

      CLASS ltc_validate_customer IMPLEMENTATION.

         METHOD class_setup.
            mo_osql_env = cl_osql_test_environment=>create(
               i_dependency_list = VALUE #( ( '/DMO/CUSTOMER' ) ) ).
         ENDMETHOD.

         METHOD class_teardown.
            mo_osql_env->destroy( ).
         ENDMETHOD.

         METHOD setup.
            mo_osql_env->clear_doubles( ).
            mo_cut = NEW ycl_travel_helper_####( ).
         ENDMETHOD.

         METHOD customer_exists.
            " Arrange – insert one customer into the mocked table
            DATA lt_customers TYPE TABLE OF /dmo/customer.
            APPEND VALUE #( customer_id = '000001' ) TO lt_customers.
            mo_osql_env->insert_test_data( lt_customers ).

            " Act
            DATA(lv_result) = mo_cut->validate_customer( iv_customer_id = '000001' ).

            " Assert
            cl_abap_unit_assert=>assert_equals(
               act = lv_result
               exp = abap_true
               msg = 'Customer 000001 should be found' ).
         ENDMETHOD.

         METHOD customer_not_exists.
            " Arrange – table stays empty

            " Act
            DATA(lv_result) = mo_cut->validate_customer( iv_customer_id = '999999' ).

            " Assert
            cl_abap_unit_assert=>assert_equals(
               act = lv_result
               exp = abap_false
               msg = 'Customer 999999 should not be found' ).
         ENDMETHOD.

         METHOD initial_customer.
            cl_abap_unit_assert=>assert_equals(
               act = mo_cut->validate_customer( iv_customer_id = '' )
               exp = abap_false
               msg = 'An initial customer ID must be rejected' ).
         ENDMETHOD.

      ENDCLASS.
      ```
   </details>
</details>

---

## Übung 5.2: Ablehnung vor dem Datenbankschreibzugriff prüfen
[↑ Zum Seitenanfang](#)

> Teste neben der Hilfsklasse auch den Service: Ein negatives Ergebnis der Hilfsklasse muss eine Datenbankänderung verhindern.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne `YCL_TRAVEL_SERVICE_####` und bitte Copilot:

   ```text
   Erzeuge lokale ABAP Unit-Tests für save_travel in YCL_TRAVEL_SERVICE_####.
   Verwende CL_OSQL_TEST_ENVIRONMENT für YTRAVEL#### und /DMO/CUSTOMER.
   Befülle und leere ausschließlich Doubles; zerstöre die Testumgebung in class_teardown.
   Rufe load_demo_data nicht auf, verwende keine echten Datenbankdaten und führe
   weder COMMIT noch ROLLBACK aus. Decke folgende Fälle ab:
   - Vorhandener Kunde: success ist wahr, und die eingefügte Zeile entspricht der Eingabe.
   - Vorhandene Reise, fehlender Kunde: success ist falsch. Lies die Zeile unmittelbar
     nach save_travel und prüfe, dass die vollständige Zeile dem vorbereiteten Ausgangswert entspricht.
   - Neue Reise, fehlender Kunde: success ist falsch, und es wurde keine Reisezeile eingefügt.
   - Initiale Kunden-ID: success ist falsch, und es wurde keine Zeile eingefügt.
   - Initiale Reise-ID bei gültigem Kunden: success ist falsch, und es wurde keine Zeile eingefügt.
   Prüfe aussagekräftige Validierungsmeldungen ebenso wie boolesche Werte.
   Zeige die Tests vor Aktivierung und Ausführung zur Prüfung.
   ```

2. Prüfe, dass die Servicetests das Tabellen-Double **vor jeglichem Aufräumen** abfragen. Eine beispielhafte Folge von Assertions für eine vorhandene Reise ist:

   ```abap
   " Arrange: ls_before is a travel row already inserted into the SQL double.
   " The /DMO/CUSTOMER double contains only its valid customer.
   DATA(ls_invalid) = ls_before.
   ls_invalid-customer_id = '999999'. " Absent from this test's customer double
   DATA(ls_result) = mo_cut->save_travel( ls_invalid ).
   cl_abap_unit_assert=>assert_false( act = ls_result-success ).

   SELECT SINGLE FROM ytravel#### FIELDS *
     WHERE travel_id = @ls_before-travel_id
     INTO @DATA(ls_after).
   cl_abap_unit_assert=>assert_equals( act = sy-subrc exp = 0 ).
   cl_abap_unit_assert=>assert_equals( act = ls_after exp = ls_before ).
   ```

   Dies ist ein Ausschnitt innerhalb einer erzeugten Testmethode. Lass Copilot die Deklarationen der Testdaten sowie Setup und Teardown ergänzen. Beide Zeilen müssen denselben Wert für den aktuellen Mandanten enthalten.

3. Aktiviere nach der Prüfung die Tests für `YCL_TRAVEL_HELPER_####` und `YCL_TRAVEL_SERVICE_####` und führe sie aus. Erwarte mindestens drei erfolgreiche Tests für die Hilfsklasse und fünf für den Service. Die Testausführung muss Teilnehmer- und `/DMO/`-Daten unverändert lassen.
4. Falls ein Test fehlschlägt, lass Copilot erklären, ob der Fehler in der Implementierung oder im Testaufbau liegt. Prüfe die vorgeschlagene Korrektur und führe die Tests beider Klassen erneut aus.

</details>

---

## Zusammenfassung
[↑ Zum Seitenanfang](#)

Du hast die Pflichtübungen abgeschlossen. Du hast Tests für Hilfsklasse und Service erzeugt und geprüft, die Validierung vor dem Datenbankschreibzugriff verifiziert und Testergebnisse für gezielte Korrekturen verwendet.

### Was du in diesem Workshop entwickelt hast

Im Verlauf des Workshops hast du:

1. VS Code mit einem ABAP-System verbunden und den ADT MCP Server aktiviert.
2. Mit GitHub Copilot Travel- und Booking-Tabellen sowie normale ABAP-Klassen angelegt.
3. Beispieldaten geladen und über eine ausführbare Konsolenklasse angezeigt.
4. Eine Kundenvalidierung vor dem Datenbankschreibzugriff ergänzt und den abgelehnten Pfad geprüft.
5. Isolierte ABAP Unit-Tests für Hilfsklasse und Service erzeugt und ausgeführt.

Fahre optional mit **[Übung 6: ABAP-Code in Visual Studio Code debuggen](../ex06/README.de.md)** oder **[Übung 7: Einen eigenen Agenten erstellen](../ex07/README.de.md)** fort.

**[Zurück zur Workshop-Startseite](../../README.de.md)**
