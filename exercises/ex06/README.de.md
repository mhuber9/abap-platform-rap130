[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 6: ABAP-Code in Visual Studio Code debuggen _(Optional)_

## Einführung

Du debuggst die ausführbare Travel-Anwendung aus [Übung 4](../ex04/README.de.md). Verfolge den Aufruf von der Konsolenklasse zu `save_travel` und `validate_customer` und untersuche, warum ein Kunde akzeptiert und ein anderer abgelehnt wird.

### Übungen

- [6.1 – Breakpoints in Service und Hilfsklasse setzen](#übung-61-breakpoints-in-service-und-hilfsklasse-setzen)
- [6.2 – Den Debugger auslösen und verbinden](#übung-62-den-debugger-auslösen-und-verbinden)
- [6.3 – Variablen prüfen und den Code schrittweise durchlaufen](#übung-63-variablen-prüfen-und-den-code-schrittweise-durchlaufen)
- [6.4 – Watch und Call Stack verwenden](#übung-64-watch-und-call-stack-verwenden)
- [Zusammenfassung](#zusammenfassung)

> Ersetze `####` durch deine vierstellige Gruppen-ID. Schließe zuerst Übung 4 ab und aktiviere vor dem Debuggen alle Quellcodeänderungen.

---

## Übung 6.1: Breakpoints in Service und Hilfsklasse setzen
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne `YCL_TRAVEL_SERVICE_####` mit **ABAP: Open Object**.
2. Navigiere über Outline oder **Go to Symbol in Editor** zu `save_travel`.
3. Klicke neben dem Aufruf der Hilfsklasse in den linken Editorrand, um einen Breakpoint zu setzen.
4. Setze einen weiteren Breakpoint auf `MODIFY ytravel#### FROM @is_travel`.
5. Öffne `YCL_TRAVEL_HELPER_####` und setze einen Breakpoint auf die Anweisung `SELECT SINGLE`.
6. Öffne **Run & Debug** über die Activity Bar oder mit **Ctrl+Shift+D** (macOS: **Cmd+Shift+D**) und prüfe, ob alle drei Einträge im Panel **Breakpoints** erscheinen.

> Der erste Breakpoint kennzeichnet jeden Speicherversuch. Der Breakpoint am SQL-Schreibzugriff zeigt, ob dieser Versuch die Datenbankänderung erreicht.

</details>

---

## Übung 6.2: Den Debugger auslösen und verbinden
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne `YCL_TRAVEL_APP_####`. Stelle sicher, dass die festen Reise- und Kundenkonstanten weiterhin zu deinen Beispieldaten passen und die ungültige Kunden-ID nicht existiert.
2. Führe über die Command Palette **ABAP: Run ABAP Application (Console)** aus. Verwende dieselbe Destination und denselben Benutzer wie beim Setzen der Breakpoints.
3. Falls ADT um Erlaubnis bittet, sich mit der ABAP-Debugging-Sitzung zu verbinden, bestätige dies. Der Editor sollte beim Aufruf der Hilfsklasse im Service für den gültigen Speicherversuch anhalten.
4. Öffne nach dem Anhalten bei Bedarf **Run & Debug**. Die Debugger-Panels und die Debug-Toolbar sind jetzt vollständig sichtbar. Mache dich mit den Panels vertraut:

   | Panel | Zweck |
   |-------|---------|
   | Variables | Werte im ausgewählten Aufrufkontext |
   | Watch | Ausdrücke, die du wiederholt prüfen möchtest |
   | Call Stack | Methodenaufrufe bis zur aktuellen Anweisung |
   | Breakpoints | Aktive und deaktivierte Breakpoints |

5. Suche **Continue**, **Step Over**, **Step Into**, **Step Out** und **Stop** in der Debug-Toolbar. Die standardmäßigen VS-Code-Tastenkombinationen lauten entsprechend F5, F10, F11, Shift+F11 und Shift+F5. Nutze die Toolbar, falls deine Tastenzuordnung abweicht. Diese Funktionen verwendest du im nächsten Abschnitt.
6. Prüfe vor den Einzelschritten, ob die aktuelle Quellcodeanweisung und das Panel **Variables** sichtbar sind.

Weitere Informationen zur Oberfläche findest du in der [VS-Code-Debugging-Dokumentation](https://code.visualstudio.com/docs/debugtest/debugging); ABAP-Werkzeuge beschreibt das [ADT-Tutorial von SAP](https://developers.sap.com/tutorials/abap-environment-adt-coretools-vscode).

Falls die Ausführung ohne Unterbrechung endet, prüfe, ob die Breakpoints aktiviert und gebunden sind, der Quellcode aktiv ist und die ausgewählte Klasse `save_travel` erreicht. Kontrolliere Destination und Benutzer erneut. Falls deine Backend- oder ADT-Version keine Verbindung zulässt, notiere die Version und bitte die Kursleitung, Debugger-Unterstützung und Berechtigungen zu prüfen. Die Konsolen- und Unit-Test-Übungen bleiben nutzbar.

</details>

---

## Übung 6.3: Variablen prüfen und den Code schrittweise durchlaufen
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Klappe am ersten Breakpoint im Service `is_travel` auf. Vergleiche die Reise- und Kunden-ID mit den gültigen Konstanten in der ausführbaren Klasse.
2. Wechsle mit **Step Into** nach `validate_customer`. Prüfe `iv_customer_id`.
3. Führe die Datenbankabfrage mit **Step Over** aus und prüfe `rv_exists`. Für den vorhandenen Kunden sollte der Wert wahr sein.
4. Kehre mit **Step Out** zum Service zurück und fahre bis zum `MODIFY`-Breakpoint fort. Dieser gültige Aufruf muss den Schreibzugriff erreichen.
5. Fahre bis zum nächsten Aufruf der Hilfsklasse fort. Dies ist der Versuch mit dem ungültigen Kunden. Prüfe, dass sich die Kunden-ID unterscheidet, während die Reise-ID gleich bleibt.
6. Durchlaufe die Abfrage schrittweise. `rv_exists` sollte falsch sein. Verfolge die Zuweisung der Fehlermeldung und den frühen Rücksprung.
7. Prüfe, dass der ungültige Versuch den `MODIFY`-Breakpoint niemals erreicht. Untersuche im Aufrufer das negative Erfolgsergebnis und den Vergleich auf eine unveränderte Zeile vor dem Rollback.

> Ändere Kundenwerte nicht im Debugger. Verwende die festen Beispiele, damit die Konsolenergebnisse reproduzierbar bleiben.

</details>

---

## Übung 6.4: Watch und Call Stack verwenden
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Füge während einer Unterbrechung im Service `is_travel-customer_id` und `rs_result-success` zu **Watch** hinzu.
2. Prüfe während einer Unterbrechung in der Hilfsklasse `iv_customer_id` und `rv_exists`. Ausdrücke aus einem anderen Aufrufkontext können nicht verfügbar sein. Wähle dann den passenden Kontext, statt dies als Programmfehler zu deuten.
3. Prüfe den **Call Stack**. Die Aufrufe deiner Anwendung sollten diesen Pfad zeigen:

   ```text
   YCL_TRAVEL_APP_####     IF_OO_ADT_CLASSRUN~MAIN
     YCL_TRAVEL_SERVICE_####  SAVE_TRAVEL
       YCL_TRAVEL_HELPER_####   VALIDATE_CUSTOMER
   ```

4. Wähle den Aufrufkontext der aufrufenden Methode, um die ursprünglichen Reisedaten zu sehen, und wechsle anschließend zurück zur Hilfsklasse.
5. Lasse die Ausführung regulär bis zum Ende weiterlaufen, damit der Aufrufer die beschriebenen Commit-/Rollback-Grenzen erreicht. Prüfe die abschließenden Konsolenmeldungen.
6. Deaktiviere oder entferne zum Schluss deine Breakpoints.

</details>

---

## Zusammenfassung
[↑ Zum Seitenanfang](#)

Du hast den ABAP-Debugger über eine Konsolenanwendung gestartet, Methodenaufrufe verfolgt, Variablen und Watches geprüft und verifiziert, dass der abgelehnte Pfad vor dem Datenbankschreibzugriff zurückkehrt.

Weiter geht es mit **[Übung 7: Einen eigenen Agenten erstellen](../ex07/README.de.md)** oder zurück zur **[Workshop-Startseite](../../README.de.md)**.
