[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 3: Die Travel-Anwendung ausführen

## Einführung

In [Übung 2](../ex02/README.de.md) hast du die Travel-Tabellen und Klassen angelegt. Jetzt lädst du einen kleinen Beispieldatensatz, zeigst Reisen mit ihren Buchungen in der ABAP Console an und erstellst die Hilfsklasse zur Kundenvalidierung für die nächste Übung.

### Übungen

- [3.1 – Beispieldaten für Reisen und Buchungen laden](#übung-31-beispieldaten-für-reisen-und-buchungen-laden)
- [3.2 – Konsolenausgabe ausführen und prüfen](#übung-32-konsolenausgabe-ausführen-und-prüfen)
- [3.3 – Die Hilfsklasse zur Kundenvalidierung anlegen](#übung-33-die-hilfsklasse-zur-kundenvalidierung-anlegen)
- [Zusammenfassung und nächste Übung](#zusammenfassung-und-nächste-übung)

> Verwende das Paket `$TMP` und ersetze `####` in Klassen- und Tabellennamen durch deine vierstellige Gruppen-ID. Schreibe ausschließlich in deine Teilnehmertabellen.

---

## Übung 3.1: Beispieldaten für Reisen und Buchungen laden
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne `YCL_TRAVEL_SERVICE_####` und sende Copilot mit **`abap-developer`** diesen Prompt:

   ```text
   Implementiere load_demo_data in YCL_TRAVEL_SERVICE_####.
   Falls YTRAVEL#### oder YBOOKING#### im aktuellen Mandanten bereits Daten enthält,
   gib 'Demo data already present; loading skipped' zurück und ändere nichts.
   Lies andernfalls bis zu fünf Zeilen aus /DMO/TRAVEL, sortiert nach travel_id,
   für die mindestens eine /DMO/BOOKING-Zeile und ein vorhandener, nicht initialer
   Kunde in /DMO/CUSTOMER existieren. Lies nur Buchungen zu diesen ausgewählten Reisen.
   Falls keine passenden Quellreisen existieren, gib 'No demo source data found'
   zurück und schreibe nichts.
   Übertrage die Quellzeilen anhand der in Übung 2 untersuchten Definitionen in unsere Tabellen.
   Berücksichtige unterschiedliche Feldnamen, Statuswerte und Audit-Felder ausdrücklich.
   Gehe nicht davon aus, dass CORRESPONDING allein alle Felder korrekt zuordnet.
   Befülle bei Bedarf den aktuellen Mandanten.
   Füge Reisen vor Buchungen ein. Führe in dieser Methode weder Commit noch Rollback aus.
   Reiche Datenbankausnahmen an den Aufrufer weiter, damit er den gesamten Ladevorgang
   zurückrollen kann. Gib bei Erfolg 'Demo data loaded' zurück.
   Lösche oder aktualisiere niemals /DMO/-Daten.

   Passe main in YCL_TRAVEL_APP_#### an: Rufe load_demo_data einmal auf und führe
   bei normaler Rückkehr COMMIT WORK aus. Fange cx_sy_open_sql_db ab, führe bei einem
   Fehler ROLLBACK WORK aus, gib den Fehler aus und beende die Methode.
   Gib anschließend die Lademeldung, die Reiseanzahl und jede Reise mit ihren Buchungen
   über read_travels, read_bookings und out->write aus.
   Falls keine Reisen existieren, gib 'No travel data available' aus und beende die Methode.
   Belasse sämtliche SELECT-/INSERT-Anweisungen im Service.
   Zeige die Änderungen vor der Aktivierung zur Prüfung. Führe die Klasse noch nicht aus.
   ```

2. Prüfe die Feldzuordnung. Die Quelltabelle für Reisen kann `status` verwenden, während die Referenzstruktur `overall_status` verwendet. Lass Copilot die in deinem System gefundene Zuordnung erklären. Prüfe, dass die Buchungsauswahl nicht versehentlich die gesamte Quelltabelle liest, wenn keine Reisen ausgewählt wurden.
3. Prüfe, dass ein zweiter Lauf das Laden überspringt, alle Schreibzugriffe dein Suffix verwenden und der Aufrufer einen fehlgeschlagenen Ladevorgang vor dem Verlassen der Methode zurückrollt.
4. Bestätige die Aktivierung der Serviceklasse und der ausführbaren Klasse.

</details>

---

## Übung 3.2: Konsolenausgabe ausführen und prüfen
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne `YCL_TRAVEL_APP_####` im Editor.
2. Öffne die Command Palette und wähle **ABAP: Run ABAP Application (Console)**. Wähle bei Bedarf die Klasse. Verwende den Befehl zur Konsolenausführung deiner ADT-Version, nicht die Ausführung von ABAP Unit-Tests.

   Dieser Befehl ist in der [SAP-Dokumentation zur Konsolenausführung](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/testing-and-quality-checking) beschrieben.

3. Prüfe das Panel **Output** mit der ABAP-Konsolenausgabe. Darstellung und IDs hängen von deinen Quelldaten ab. Die Ausgabe sollte diese Bereiche enthalten:

   ```text
   Demo data loaded
   Travel count: <1 to 5>
   Travel: <travel_id>, Customer: <customer_id>, ...
   Bookings for travel <travel_id>: <booking rows>
   ...
   ```

4. Prüfe, dass die `travel_id` jeder Buchung zur darüber ausgegebenen Reise passt. Notiere eine Reise-ID und die zugehörige gültige Kunden-ID für Übung 4.
5. Führe die Klasse erneut aus. Erwarte `Demo data already present; loading skipped` bei unveränderter Anzahl der Reisen und Buchungen. Der Ladevorgang darf weder Datensätze duplizieren noch spätere Änderungen zurücksetzen.
6. Wenn `No demo source data found` erscheint, bitte die Kursleitung, Daten für das Flight Reference Scenario bereitzustellen. Wird das Laden übersprungen, aber keine Reise angezeigt, prüfe deine Teilnehmertabellen auf unvollständige Daten eines früheren Versuchs. Kläre dies vor dem Fortfahren mit der Kursleitung. Leere keine gemeinsam genutzten Quelltabellen.

> ✅ **Erfolg:** Die Konsolenausgabe zeigt Reisen und Buchungen an. Ein erneuter Lauf erhält die Daten.

</details>

---

## Übung 3.3: Die Hilfsklasse zur Kundenvalidierung anlegen
[↑ Zum Seitenanfang](#)

> Diese Hilfsklasse prüft, ob ein Kunde existiert. Das Laden von Beispieldaten gehört zum zuvor angelegten Service.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Wähle in der Command Palette **ABAP: Create New ABAP Object** und dann **Class**.
2. Gib das Paket `$TMP`, den Namen `YCL_TRAVEL_HELPER_####` und die Beschreibung `Travel customer validation ####` ein. Lass Superklasse und Interface leer.
3. Ersetze den Klassenquellcode durch Folgendes und setze dabei deine Gruppen-ID ein:

   ```abap
   CLASS ycl_travel_helper_#### DEFINITION
     PUBLIC FINAL CREATE PUBLIC.
     PUBLIC SECTION.
       METHODS validate_customer
         IMPORTING iv_customer_id TYPE /dmo/customer_id
         RETURNING VALUE(rv_exists) TYPE abap_bool.
   ENDCLASS.

   CLASS ycl_travel_helper_#### IMPLEMENTATION.
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

4. Prüfe den frühen Rücksprung bei einer initialen Kunden-ID und den ausschließlich lesenden Datenbankzugriff. Speichere und aktiviere die Hilfsklasse.

</details>

---

## Zusammenfassung und nächste Übung
[↑ Zum Seitenanfang](#)

Du hast Beispieldaten in die Teilnehmertabellen geladen, die ausführbare Klasse gestartet, die Konsolenausgabe für Reisen und Buchungen geprüft und eine wiederverwendbare Hilfsklasse zur Kundenvalidierung angelegt.

Weiter geht es mit **[Übung 4: Eine Validierung ergänzen](../ex04/README.de.md)**.
