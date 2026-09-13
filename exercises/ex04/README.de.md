[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 4: Eine Validierung ergänzen

## Einführung

In [Übung 3](../ex03/README.de.md) hast du `YCL_TRAVEL_HELPER_####->validate_customer` angelegt. Jetzt bittest du Copilot, diese Methode vor dem Speichern einer Reise aus dem Service aufzurufen. Die Konsolenanwendung demonstriert einen erfolgreichen und einen abgelehnten Speichervorgang.

### Übungen

- [4.1 – Die Validierung definieren und implementieren](#übung-41-die-validierung-definieren-und-implementieren)
- [4.2 – Die erweiterte Travel-Anwendung ausführen und testen](#übung-42-die-erweiterte-travel-anwendung-ausführen-und-testen)
- [Zusammenfassung und nächste Übung](#zusammenfassung-und-nächste-übung)

> Ersetze `####` durch deine vierstellige Gruppen-ID. Prüfe erzeugte Änderungen vor der Aktivierung.

---

## Übung 4.1: Die Validierung definieren und implementieren
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne `YCL_TRAVEL_SERVICE_####` und GitHub Copilot Chat. Wähle anschließend **`abap-developer`** in der Agentenauswahl.
2. Gib folgenden Prompt ein:

   ```text
   Ergänze die Kundenvalidierung in save_travel in YCL_TRAVEL_SERVICE_####.
   Behalte die vorhandene Signatur und ty_result (success und message) bei.
   Prüfe zunächst, dass travel_id nicht initial ist, und rufe anschließend
   validate_customer von YCL_TRAVEL_HELPER_#### mit is_travel-customer_id auf.
   Falls die Hilfsklasse abap_false zurückgibt, gib success = abap_false und
   'Customer <ID> does not exist' zurück, beziehungsweise 'Customer ID is required'
   bei einer initialen ID. Kehre vor jedem INSERT, UPDATE oder MODIFY zurück.
   Schreibe keine ungültigen Daten.
   Bei gültiger Eingabe speichere die Reise in YTRAVEL#### und liefere das tatsächliche SQL-Ergebnis.
   Führe hier weder Commit noch Rollback aus; die ausführbare Klasse steuert die Transaktion.
   Bitte mich vor der Aktivierung um Prüfung der Änderungen.
   ```

3. Prüfe die erzeugte Methode. Ihr Kontrollfluss sollte diesem Beispiel entsprechen:

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

4. Stelle sicher, dass der abgelehnte Pfad vor `MODIFY` zurückkehrt und weder die Hilfsklasse noch der Service einen Commit ausführt. Datenbankausnahmen behandelt die ausführbare aufrufende Klasse.
5. Gib nach der Quellcodeprüfung die Aktivierung frei und prüfe das Panel **Problems**.

</details>

---

## Übung 4.2: Die erweiterte Travel-Anwendung ausführen und testen
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Verwende die Reise-ID und die vorhandene Kunden-ID, die du in Übung 3 notiert hast. Verwende `999999` als Kunden-ID für das Beispiel mit ungültiger Eingabe.
2. Öffne `YCL_TRAVEL_APP_####` und sende den folgenden Prompt, nachdem du die Werte in spitzen Klammern sowie `####` ersetzt hast:

   ```text
   Erweitere YCL_TRAVEL_APP_#### nach dem vorhandenen Laden und Anzeigen der Beispieldaten.
   Verwende feste Konstanten für Reise-ID <travel ID>, gültige Kunden-ID <existing ID>
   und ungültige Kunden-ID '999999'. Ergänze keine interaktive Eingabe.
   Lies die ausgewählte Reise über read_travels. Falls sie fehlt, gib eine klare Meldung
   aus und beende die Methode ohne Speichern. Bewahre die gesamte Zeile als Ausgangswert auf.

   Setze zunächst customer_id auf die gültige ID und description auf 'Copilot validation demo'.
   Rufe save_travel auf. Gib success und message aus dem Ergebnis aus.
   Führe nur bei Erfolg COMMIT WORK aus; andernfalls ROLLBACK WORK, gib den Fehler aus
   und beende die Methode. Lies die gespeicherte Reise erneut und bewahre die vollständige
   Zeile als Vergleichsbasis auf.

   Kopiere die Vergleichsbasis und ändere nur customer_id auf die ungültige ID.
   Rufe save_travel auf und gib das Ergebnis aus. Lies die Reise unmittelbar nach dem Aufruf,
   vor dem Rollback, und vergleiche die vollständige Zeile mit der Vergleichsbasis.
   Gib aus, ob sie unverändert ist.
   Führe nach diesem absichtlich ungültigen Versuch ROLLBACK WORK aus, selbst wenn der Service
   unerwartet Erfolg meldet. Gib einen ausdrücklichen Fehler aus, wenn die Eingabe akzeptiert
   wurde oder sich die gespeicherte Zeile geändert hat. Lies die endgültig gespeicherte Reise
   erneut und zeige sie an.

   Fange cx_sy_open_sql_db bei den Speicheroperationen ab, rolle zurück, gib den Fehler aus
   und beende die Methode. Belasse SQL im Service. Zeige den Code vor der Aktivierung zur
   Prüfung und führe ihn noch nicht aus.
   ```

3. Prüfe die Transaktionsgrenzen und bestätige die Aktivierung. Führe die ausführbare Klasse mit **ABAP: Run ABAP Application (Console)** aus.
4. Erwarte folgende Ergebnisse; IDs und Darstellung boolescher Werte können abweichen:

   ```text
   Demo data already present; loading skipped
   ... Travel and Booking output ...
   Valid save: success = X; Travel <ID> saved
   Invalid save: success = <false>; Customer 999999 does not exist
   Stored row unchanged before rollback: X
   Final travel: <original valid customer>, Copilot validation demo
   ```

Falls `999999` in deinem System existiert, verwende eine andere Kunden-ID für das ungültige Beispiel und führe es erneut aus.

5. Prüfe, dass die Buchungen weiterhin derselben Reise zugeordnet sind. Führe die Klasse erneut aus und stelle sicher, dass das Laden der Beispieldaten die gespeicherte Beschreibung nicht zurücksetzt.

> Die Prüfung der Zeile **vor dem Rollback** zeigt, dass die Validierung den Schreibzugriff verhindert hat. Eine Prüfung erst nach dem Rollback könnte einen ungültigen Schreibzugriff verdecken, der anschließend rückgängig gemacht wurde.

</details>

---

## Zusammenfassung und nächste Übung
[↑ Zum Seitenanfang](#)

Du hast mit Copilot eine Hilfsklasse in eine normale ABAP-Speichermethode eingebunden, die Änderung geprüft und erfolgreiche sowie abgelehnte Speichervorgänge anhand der Konsolenausgabe verifiziert.

Weiter geht es mit **[Übung 5: ABAP Unit-Tests erzeugen](../ex05/README.de.md)**.
