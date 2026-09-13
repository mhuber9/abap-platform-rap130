[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 7: Einen eigenen Agenten erstellen _(Optional)_

## Einführung

Die Anwendungsübungen verwenden **`abap-developer`**. In dieser optionalen Übung experimentierst du mit einem separaten eigenen Agenten, ohne diese Konfiguration zu ersetzen.

Ein eigener Agent gibt Copilot wiederverwendbaren Kontext zur Travel-Anwendung: Objektnamen, Paket, ABAP-Cloud-Konventionen und den Prüfablauf. Du erstellst einen solchen Agenten und prüfst, ob er deine Anwendung versteht, ohne jedes Detail wiederholen zu müssen.

### Übungen

- [7.1 – Einen eigenen Agenten in GitHub Copilot erstellen](#übung-71-einen-eigenen-agenten-in-github-copilot-erstellen)
- [7.2 – Anweisungen für andere Coding-Agenten anpassen](#übung-72-anweisungen-für-andere-coding-agenten-anpassen)
- [Zusammenfassung](#zusammenfassung)

> Ersetze `####` vor dem Speichern der Agentenanweisungen durch deine vierstellige Gruppen-ID. Behalte `$TMP` als Paket bei.

---

## Übung 7.1: Einen eigenen Agenten in GitHub Copilot erstellen
[↑ Zum Seitenanfang](#)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne Copilot Chat und verwende **Configure Custom Agents** oder führe **Chat: New Custom Agent** über die Command Palette aus.
2. Wähle einen Speicherort im Workspace und nenne den Agenten `travel-workshop`. Speichere ihn als `.github/agents/travel-workshop.agent.md` in einem lokalen Workspace-Ordner. Falls dein Workspace nur die virtuelle ADT-Destination enthält, wähle im Erstellungsdialog stattdessen einen Speicherort auf Benutzerebene.
3. Füge diese Definition ein und ersetze `####` durch deine vierstellige Gruppen-ID:

   ```markdown
   ---
   name: Travel Workshop
   description: Entwickle die ABAP-Travel-Konsolenanwendung mit ADT und Copilot.
   ---

   Du bist ABAP-Entwickler und erstellst eine Konsolenanwendung für Reisen und Buchungen
   in Visual Studio Code mit GitHub Copilot und dem ADT MCP Server.

   ## Anwendungskontext
   - Mein vierstelliges Teilnehmersuffix ist ####. Verwende das vorhandene lokale Paket $TMP.
   - Lege für Workshop-Objekte weder ein neues Paket noch einen Transportauftrag an.
   - Tabellen: YTRAVEL#### und YBOOKING#### auf Basis von /DMO/TRAVEL_DATA und /DMO/BOOKING_DATA.
   - YCL_TRAVEL_APP_#### implementiert IF_OO_ADT_CLASSRUN und gibt Daten mit out->write aus.
   - YCL_TRAVEL_SERVICE_#### enthält load_demo_data, read_travels, read_bookings und save_travel.
   - YCL_TRAVEL_HELPER_#### stellt validate_customer bereit.

   ## Arbeitsregeln
   - Untersuche vor Änderungen die verfügbaren ADT-Werkzeuge und den vorhandenen Quellcode.
   - Nutze unterstützte MCP-Werkzeuge für Objektanlage, Aktivierung und Unit-Test-Ausführung.
     Bearbeite Quellcode über den virtuellen ADT-Workspace. Erkläre notwendige manuelle Schritte.
   - Behalte die exakten Objektnamen und öffentlichen Methodensignaturen aus den Übungen bei.
   - Verwende normale ABAP-Cloud-Klassen und SQL. Behalte den Konsoleneinstiegspunkt bei.
   - Lies /DMO/-Quelldaten nur. Überspringe das Laden der Beispieldaten, wenn eine der Teilnehmertabellen Daten enthält.
   - Validiere Kunden vor dem Speichern. Lehne ungültige Eingaben ohne Datenbankschreibzugriff ab.
   - Belasse COMMIT WORK und ROLLBACK WORK im ausführbaren Aufrufer, außerhalb der Servicemethoden.
   - Verwende SQL-Test-Doubles für Unit-Tests mit isolierten Testdaten und ohne Commits.
   - Zeige Quellcodeänderungen vor der Aktivierung zur Prüfung und führe danach freigegebene Tests aus.
   - Berichte tatsächliche Aktivierungs- und Testergebnisse. Behaupte nicht, dass nicht ausgeführte Prüfungen bestanden wurden.
   ```

4. Speichere die Definition und wähle **Travel Workshop** in der Agentenauswahl. Prüfe, ob die ADT-Werkzeuge für diesen Agenten aktiviert sind.
5. Sende diesen ausschließlich lesenden Prüf-Prompt:

   ```text
   Fasse mein Paket, meine Objektnamen, den Einstiegspunkt und die Transaktionsregeln zusammen.
   Untersuche save_travel und erkläre, wie ungültige Kunden-IDs abgelehnt werden.
   Ändere und aktiviere nichts.
   ```

6. Prüfe, ob Copilot deine Objekte mit dem richtigen Suffix nennt und den frühen Rücksprung vor dem SQL-Schreibzugriff erklärt. Falls nicht, kontrolliere den ausgewählten Agenten und ob deine Definition geladen wurde.
7. Wähle anschließend wieder **`abap-developer`**, bevor du zu den Anwendungsübungen zurückkehrst.

Die offizielle [VS-Code-Dokumentation zu eigenen Agenten](https://code.visualstudio.com/docs/agent-customization/custom-agents) beschreibt unterstützte Speicherorte und Befehle zur Erstellung.

</details>

---

## Übung 7.2: Anweisungen für andere Coding-Agenten anpassen
[↑ Zum Seitenanfang](#)

> Optionale Alternative für Teilnehmer, die bereits einen anderen kompatiblen Coding-Agenten verwenden.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Prüfe, ob dein Agent die ADT-MCP-Verbindung **und** das Lesen und Bearbeiten des virtuellen ADT-Workspace unterstützt. MCP-Konnektivität allein reicht für diesen Ablauf nicht aus.
2. Übernimm den Anwendungskontext und die Arbeitsregeln aus Abschnitt 7.1 in den vom Agenten dokumentierten Anweisungsmechanismus. Dateinamen und Formate unterscheiden sich. Gehe nicht davon aus, dass Copilots `.agent.md`-Format übertragbar ist.
3. Aktiviere die relevanten ADT-Werkzeuge, lade die Anweisungen und führe denselben ausschließlich lesenden Prüf-Prompt aus.
4. Prüfe, dass der Agent tatsächlichen ABAP-Quellcode liest und dein Teilnehmersuffix verwendet, bevor du Änderungen anforderst.

</details>

---

## Zusammenfassung
[↑ Zum Seitenanfang](#)

Du hast wiederverwendbare Anweisungen für die Travel-Anwendung konfiguriert und geprüft, ob dein Agent die Regeln zu Paket, Namensgebung, Transaktionen und Prüfung anwenden kann.

**[Zurück zur Workshop-Startseite](../../README.de.md)**
