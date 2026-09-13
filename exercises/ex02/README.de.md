[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 2: Die ABAP-Travel-Anwendung erzeugen

## Einführung

In [Übung 1](../ex01/README.de.md) hast du GitHub Copilot mit den ADT-MCP-Werkzeugen verbunden. Jetzt legst du mit natürlichsprachlichen Anweisungen normale ABAP-Repository-Objekte für das im Workshop verwendete Szenario mit Reisen und Buchungen an.

Du lernst, Objektnamen und Anforderungen vorzugeben, den vorhandenen Quellcode zu untersuchen, erzeugten Code zu prüfen und voneinander abhängige Objekte zu aktivieren. Copilot schreibt den Anwendungsquellcode; ADT legt die Repository-Objekte an und aktiviert sie.

### Übungen

- [2.1 – Das vorhandene lokale Paket öffnen](#übung-21-das-vorhandene-lokale-paket-öffnen)
- [2.2 – Die Travel-Tabellen und Klassen erzeugen](#übung-22-die-travel-tabellen-und-klassen-erzeugen)
- [Zusammenfassung und nächste Übung](#zusammenfassung-und-nächste-übung)

> Verwende das Paket `$TMP` und ersetze `####` in Objektnamen durch deine vierstellige Gruppen-ID. Prüfe KI-generierten Quellcode vor der Aktivierung und korrigiere Fehler mit Copilot. Übernimm Code nicht allein deshalb, weil er erzeugt wurde.

---

## Übung 2.1: Das vorhandene lokale Paket öffnen
[↑ Zum Seitenanfang](#)

> Verwende für alle Objekte dieses Workshops das vorhandene lokale Paket **`$TMP`**.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne in **Visual Studio Code** die Command Palette mit **Ctrl+Shift+P** (macOS: **Cmd+Shift+P**).
2. Wähle **ABAP: Add Package as Folder to Workspace** und bei Bedarf deine verbundene ABAP-Destination.
3. Gib **`$TMP`** ein und füge es dem Workspace hinzu. Falls es bereits vorhanden ist, öffne den bestehenden Ordner.
4. Verwende beim Anlegen jeder Tabelle und Klasse das Paket `$TMP`. Lege für diese lokalen Workshop-Objekte weder ein neues Paket noch einen Transportauftrag an.
5. Prüfe wie unter [Erste Schritte](../ex0/README.de.md#übung-01-deine-gruppen-id-festlegen) beschrieben, ob dein vierstelliges Suffix bereits verwendet wird. Auch andere Teilnehmer können `$TMP` verwenden.

</details>

---

## Übung 2.2: Die Travel-Tabellen und Klassen erzeugen
[↑ Zum Seitenanfang](#)

> Lege zwei Datenbanktabellen, eine Serviceklasse und eine ausführbare Klasse in deinem Paket an.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

### Schritt 1: Die Referenzdaten untersuchen

Die Referenzobjekte befinden sich im Paket **`/DMO/FLIGHT_LEGACY`** deines ABAP-Systems. Öffne dieses Paket, um ihre Definitionen zu untersuchen.

1. Öffne **GitHub Copilot Chat** und wähle **`abap-developer`** in der Agentenauswahl.
2. Gib diesen Prompt ein:

   ```text
   Lies /DMO/TRAVEL_DATA und /DMO/BOOKING_DATA sowie die Quelltabellen
   /DMO/TRAVEL und /DMO/BOOKING im verbundenen ABAP-System.
   Diese Referenzobjekte befinden sich im Paket /DMO/FLIGHT_LEGACY.
   Erkläre die Schlüssel der Travel-Booking-Beziehung, Kunden- und Währungsfelder
   sowie Unterschiede bei Feldnamen oder Statuswerten zwischen Strukturen und Tabellen.
   Wir entwickeln eine einfache ABAP-Travel-Konsolenanwendung. Ändere noch nichts.
   ```

3. Prüfe die zurückgegebenen Definitionen. `travel_id` identifiziert eine Reise. Eine Buchung gehört zu dieser Reise und wird innerhalb des Mandanten durch `travel_id` zusammen mit `booking_id` identifiziert.

### Schritt 2: Die Anwendung anlegen

Teile **`abap-developer`** vor dem Erstellungs-Prompt deine Gruppen-ID im Chat mit. Ersetze `<your_id>` durch deine vierstellige ID, zum Beispiel `0123`:

```text
Meine Gruppen-ID ist <your_id>. Verwende diesen Wert überall dort, wo #### in Objektnamen vorkommt, und behalte führende Nullen bei.
```

4. Ersetze `####` im folgenden Prompt durch deine vierstellige Gruppen-ID. Lass `$TMP` unverändert und sende den Prompt mit **`abap-developer`**:

   ```text
   Erstelle eine einfache ABAP-Cloud-Anwendung für Travel und Booking im vorhandenen lokalen Paket $TMP.
   Lege weder ein Paket noch einen Transportauftrag an.
   Verwende unterstützte ADT-Werkzeuge zur Objektanlage und bearbeite den Quellcode
   im virtuellen ADT-Workspace. Falls ein Objekttyp mit den verfügbaren Werkzeugen
   nicht angelegt werden kann, nenne mir das Objekt, das ich mit
   ABAP: Create New ABAP Object anlegen soll, und fahre danach fort.

   Lege diese Objekte mit genau diesen Namen an:
   - YTRAVEL####: mandantenabhängige transparente Tabelle auf Basis von /DMO/TRAVEL_DATA.
     Schlüssel: client und travel_id. Behalte die fachlichen Referenzfelder der Reise bei,
     einschließlich customer_id, Datumsfeldern, Beträgen, Währung, description und overall_status.
   - YBOOKING####: mandantenabhängige transparente Tabelle auf Basis von /DMO/BOOKING_DATA.
     Schlüssel: client, travel_id, booking_id. Behalte die fachlichen Referenzfelder der Buchung bei.
     Jede Buchung muss auf eine Reise in YTRAVEL#### im selben Mandanten verweisen.
     Untersuche die tatsächlichen Strukturen; dupliziere beim Übernehmen ihrer Felder keine Schlüssel.
     Behalte die Betrags-/Währungsannotationen in beiden Tabellendefinitionen bei.
   - YCL_TRAVEL_SERVICE_####: öffentliche finale Klasse mit diesen öffentlichen Typen und Methoden:
     tt_travel = Standardtabelle von YTRAVEL#### mit leerem Schlüssel.
     tt_booking = Standardtabelle von YBOOKING#### mit leerem Schlüssel.
     ty_result = Struktur mit success TYPE abap_bool und message TYPE string.
     read_travels: Rückgabe rt_travels TYPE tt_travel, nach travel_id sortiert.
     read_bookings: Import iv_travel_id TYPE /dmo/travel_id;
       Rückgabe rt_bookings TYPE tt_booking, für diese Reise nach booking_id sortiert.
     save_travel: Import is_travel TYPE YTRAVEL####;
       Rückgabe rs_result TYPE ty_result. Lehne eine initiale travel_id ab; füge andernfalls
       die übergebene Reise mit ABAP SQL ein oder aktualisiere sie und melde das Ergebnis.
       Die Kundenvalidierung folgt in Übung 4; ergänze sie noch nicht.
     load_demo_data: Rückgabe rv_message TYPE string. Gib vorerst
       'Demo loading will be implemented in Exercise 3' zurück, ohne Daten zu schreiben.
   - YCL_TRAVEL_APP_####: öffentliche finale Klasse, die IF_OO_ADT_CLASSRUN implementiert.
     Instanziiere den Service in main, rufe read_travels auf und gib mit out->write
     'Travel application ready' sowie die Anzahl der Reisen aus. Speichere noch keine Daten.

   Belasse SQL und fachliche Operationen im Service. Die ausführbare Klasse steuert
   COMMIT WORK / ROLLBACK WORK; Servicemethoden dürfen weder Commit noch Rollback ausführen.
   Lege keine Oberfläche, Dienste, Draft-Tabellen oder Framework-Geschäftsobjekte an.
   Ändere keine /DMO/-Objekte oder -Daten. Führe die Anwendung nicht automatisch aus.
   Zeige mir den erstellten Quellcode und bitte mich vor der Aktivierung um Prüfung.
   ```

5. Falls eine manuelle Anlage erforderlich ist, öffne die Command Palette, wähle **ABAP: Create New ABAP Object**, dann **Database Table** oder **Class** und gib das oben genannte Paket und den exakten Namen ein. Öffne den erstellten Quellcode im Workspace, damit Copilot ihn weiter bearbeiten kann.

### Schritt 3: Die Objekte prüfen

6. Prüfe vor der Freigabe der Aktivierung Folgendes:

   | Objekt | Prüfschwerpunkt |
   |--------|--------------|
   | `YTRAVEL####` | Mandant und Reiseschlüssel; Kunden- und Währungsfelder |
   | `YBOOKING####` | Mandant, Reise- und Buchungsschlüssel; passende Travel-Beziehung |
   | `YCL_TRAVEL_SERVICE_####` | Öffentliche Methodenschnittstellen wie oben; SQL nur auf Teilnehmertabellen |
   | `YCL_TRAVEL_APP_####` | `IF_OO_ADT_CLASSRUN`, Serviceaufruf und `out->write` |

7. Lass Copilot die Reihenfolge der Abhängigkeiten erklären. Die Tabellen müssen vor dem Service aktiv sein, der ihre Zeilentypen verwendet. Der Service muss vor der ausführbaren Klasse aktiv sein.

   Die ausführbare Klasse sollte zu diesem Zeitpunkt so klein sein wie dieses Beispiel:

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

   Übung 3 erweitert diesen Einstiegspunkt um das Laden und die Ausgabe von Reisen und Buchungen. Übung 4 ergänzt die beiden Speicherversuche.

### Schritt 4: Aktivieren und kontrollieren

8. Bestätige nach der Prüfung die Aktivierung oder verwende für jedes Objekt **ABAP: Activate** in der Command Palette in der Reihenfolge der Abhängigkeiten.
9. Prüfe das Panel **Problems**. Falls die Aktivierung fehlschlägt, übergib Copilot die tatsächliche Fehlermeldung, prüfe die Korrektur und aktiviere erneut.
10. Aktualisiere dein Paket im Explorer und öffne jedes der vier Objekte. Stelle sicher, dass die öffentlichen Methodensignaturen dem Prompt entsprechen. Spätere Übungen verwenden genau diese Namen.

### Schritt 5: Die Travel-App ausführen

11. Öffne `YCL_TRAVEL_APP_####` im Editor und ersetze `####` durch deine vierstellige Gruppen-ID.
12. Öffne die Command Palette und wähle **ABAP: Run ABAP Application (Console)**. Wähle bei Bedarf die Klasse aus.
13. Prüfe die ABAP-Konsolenausgabe im Panel **Output**. Bei neu angelegten, leeren Tabellen erwartest du:

    ```text
    Travel application ready
    Travel count: 0
    ```

14. Prüfe, ob die Anwendung ohne Fehler endet. Eine Anzahl von null ist erwartet, da du die Beispieldaten erst in Übung 3 lädst. Falls deine Teilnehmertabelle bereits Daten enthält, erscheint stattdessen die aktuelle Anzahl der Reisen.

> ✅ **Erfolg:** Beide Tabellen und beide Klassen sind aktiv. Die Travel-App läuft erfolgreich in der Konsole. Fahre mit dem Laden der Beispieldaten in Übung 3 fort.

</details>

---

## Zusammenfassung und nächste Übung
[↑ Zum Seitenanfang](#)

Du hast das vorhandene lokale Paket `$TMP` geöffnet, Travel- und Booking-Tabellen sowie ABAP-Klassen mit Copilot erzeugt, den Quellcode geprüft, die abhängigen Objekte aktiviert und die Travel-App in der Konsole ausgeführt.

Weiter geht es mit **[Übung 3: Die Travel-Anwendung ausführen](../ex03/README.de.md)**.
