[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Übung 1: Den ADT MCP Server aktivieren 💎

## Einführung

In der vorherigen Übung hast du Visual Studio Code und die ADT-Erweiterung installiert und dich mit deinem ABAP-System verbunden (siehe [Erste Schritte](../ex0/README.de.md)).

Jetzt aktivierst du den in ADT for Visual Studio Code integrierten **ADT MCP Server** und prüfst, ob die MCP-Tools verfügbar sind.

Der ADT MCP Server stellt ABAP-Entwicklungsfunktionen als **Model Context Protocol (MCP)-Tools** bereit. Über natürlichsprachliche Prompts kannst du damit ABAP-Objekte anlegen und aktivieren sowie Unit-Tests ausführen. Weitere Informationen findest du unter [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US).

### Übungen

- [1.1 – Den ADT MCP Server in den Einstellungen aktivieren](#übung-11-den-adt-mcp-server-in-den-einstellungen-aktivieren)
- [1.2 – Prüfen, ob der MCP-Server läuft](#übung-12-prüfen-ob-der-mcp-server-läuft)
- [1.3 – Die ADT-MCP-Tools im Agenten prüfen](#übung-13-die-adt-mcp-tools-im-agenten-prüfen)
- [Zusammenfassung und nächste Übung](#zusammenfassung-und-nächste-übung)

> ℹ️ **Erinnerung:** Ersetze in den folgenden Schritten den Platzhalter **`####`** durch deine vierstellige Gruppen-ID.

---

## Über den ADT MCP Server 💎

Der **ADT MCP Server** ist ein lokaler HTTP-Server innerhalb der ADT-Erweiterung für Visual Studio Code. Er implementiert das **Model Context Protocol (MCP)**, einen offenen Standard, über den KI-Assistenten wie GitHub Copilot Tools strukturiert und authentifiziert aufrufen können.

Nach seiner Aktivierung stellt der MCP-Server ABAP-Entwicklungstools für MCP-kompatible KI-Clients bereit. In diesem Workshop verwenden wir **GitHub Copilot** als Client. Voraussetzung für andere Coding-Agenten ist die Unterstützung des virtuellen Workspace-Dateisystems von Visual Studio Code. GitHub Copilot ist hierfür bestätigt; weitere Agenten werden ebenfalls unterstützt.

**Tools in diesem Workshop – prüfe ihre Verfügbarkeit in deiner installierten Version:**

| Tool | Beschreibung |
|------|-------------|
| `abap_creation-create_object` | Legt ABAP-Entwicklungsobjekte an |
| `abap_activate-objects` | Aktiviert ABAP-Objekte im Backend-System |
| `abap_run_unit_tests` | Führt ABAP Unit-Tests aus |

Copilot bearbeitet ABAP-Quellcode über den virtuellen ADT-Workspace und verwendet verfügbare MCP-Tools für Backend-Operationen. Objektanlage und Quellcodebearbeitung sind getrennte Schritte; diese Übung verwendet keinen Anwendungsgenerator.

Eine vollständige Tool-Liste findest du unter [ADT MCP Tools](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/mcp-tools?locale=en-US).

> ⚠ **Hinweis zu KI-Ausgaben** ⚠
> Der ADT MCP Server ist eine **experimentelle Funktion**, die sich jederzeit ohne Ankündigung ändern kann. Er ist nicht für den produktiven Einsatz vorgesehen. Sichere deine Daten vor der Verwendung.

> **Weiterführende Informationen:** [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

---

## Übung 1.1: Den ADT MCP Server in den Einstellungen aktivieren
[↑ Zum Seitenanfang](#)

> Aktiviere den integrierten ADT MCP Server in den Einstellungen der Erweiterung.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne **Visual Studio Code Settings**:
   - Öffne die **Command Palette** mit **`Ctrl+Shift+P`** (macOS: **`Cmd+Shift+P`**).
   - Gib `Preferences: Open Settings (UI)` ein und wähle **Preferences: Open Settings...**.
2. Gib Folgendes in die Suchleiste ein:
   ```
   adt mcp
   ```
3. Suche die Einstellung **"Adt: Enable MCP Server"** oder eine entsprechend benannte Einstellung und aktiviere das Kontrollkästchen.

   > ℹ️ **Tipp:** Alternativ kannst du über das `{}`-Symbol oben rechts in den Settings zur JSON-Ansicht wechseln und Folgendes ergänzen:
   > ```json
   >  "adt.mcpServer.enabled": true
   > ```

4. Optional kannst du den **MCP-Server-Port** konfigurieren (Standard: `2236`). Ändere ihn nur, wenn Port 2236 auf deinem Rechner belegt ist:
   - Suche in den Settings nach `ADT MCP port`.
   - Wähle einen verfügbaren Port zwischen `0` und `65535`.
5. Lade Visual Studio Code neu, wenn du dazu aufgefordert wirst, oder schließe und starte die Anwendung erneut.

   ![Aktivierte Einstellung für den ADT MCP Server](images/ex1_mcp_setting.png)

> ⚠️ **Wichtig:** Die Systemdestination aus Übung 0.4 muss weiterhin zum Workspace hinzugefügt sein. Der MCP-Server startet erst, wenn eine Destination im Workspace aktiv ist.

</details>

---

## Übung 1.2: Prüfen, ob der MCP-Server läuft
[↑ Zum Seitenanfang](#)

> Prüfe, ob der ADT MCP Server erfolgreich gestartet wurde.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

Nach dem Aktivieren der Einstellung sollte der Server automatisch starten, sofern eine Destination im Workspace vorhanden ist.

### Methode 1: Die Startbenachrichtigung prüfen

1. Achte unten rechts in Visual Studio Code auf die Benachrichtigung:
   ```
   ADT MCP Server running on port 2236
   ```

   ![Laufender ADT MCP Server](images/ex1_mcp_running.png)

   > ℹ️ Falls du die Benachrichtigung nicht siehst, fahre mit Methode 2 fort.

### Methode 2: Über die MCP-Serverliste prüfen

1. Öffne die **Command Palette** (**`Ctrl+Shift+P`**).
2. Gib Folgendes ein:
   ```
   >MCP: List Servers
   ```
3. Wähle **"MCP: List Servers"** aus der Liste.
4. In der Auswahlliste sollte **ADT MCP Server** erscheinen.
5. Wähle den Eintrag und klicke auf **Start Server**, falls der Server noch nicht läuft.

   ![MCP-Server in der Liste und gestartet](images/ex1_mcp_list_servers.png)

### Fehlerbehebung

Falls der ADT MCP Server nicht erscheint oder nicht startet:

1. Deaktiviere **"Adt: Enable MCP Server"** und aktiviere die Einstellung erneut.
2. Entferne den Destinationsordner aus dem Workspace und füge ihn erneut hinzu: Command Palette → `ABAP: Add Destination as Folder to the Workspace...`.
3. Starte Visual Studio Code neu.

</details>

---

## Übung 1.3: Die ADT-MCP-Tools im Agenten prüfen
[↑ Zum Seitenanfang](#)

> Wähle **`abap-developer`** in GitHub Copilot Chat und prüfe, ob die ADT-MCP-Tools geladen und verfügbar sind.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne **GitHub Copilot Chat** in Visual Studio Code:
   - Klicke auf das Copilot-Symbol in der Activity Bar links oder
   - drücke **`Ctrl+Shift+I`** (macOS: **`Cmd+Shift+I`**).
2. Wähle **`abap-developer`** in der Agentenauswahl des Chats. Konfiguriere `$TMP` als Paket, `Y` als Objektpräfix und deinen verbundenen ADT MCP Server als MCP-Server. Die General- und Testing-Anweisungen legen bereits cloudkonformes ABAP, die Suche zuerst über Verzeichnisse, die Bearbeitung im VS-Code-Editor, die Ablage im Testklassen-Include und die Testausführung nach Quellcode- oder Teständerungen fest. Die Übungs-Prompts konzentrieren sich auf die Anwendungsanforderungen, statt diese Regeln zu wiederholen.

   > ℹ️ Verwende **`abap-developer`** in allen Anwendungsübungen. Falls der Eintrag fehlt, bitte die Kursleitung um die Workshop-Agentenkonfiguration, bevor du fortfährst.

3. Klicke im Eingabebereich von Copilot Chat auf **"Configure Tools"** (Tool-Symbol).
4. In der Auswahlliste der Tool-Anbieter solltest du Folgendes sehen:
   - **ADT MCP Server** mit einer Tool-Liste, zum Beispiel `abap_creation-create_object` und `abap_activate-objects`.

   > ✅ Wenn der ADT MCP Server und seine Tools angezeigt werden, ist die Einrichtung abgeschlossen.

   ![ADT-MCP-Tools in Copilot Configure Tools](images/ex1_mcp_tools_visible.png)

5. Stelle sicher, dass die ADT-MCP-Tools in der Liste ausgewählt und aktiviert sind.
6. **Tools erkunden:** Gib diesen Prompt in Copilot Chat ein:
   ```text
   Untersuche die in dieser Sitzung verfügbaren ADT-MCP-Tools. Ermittle die
   Tools zum Anlegen von ABAP-Klassen und Datenbanktabellen, zum Aktivieren
   von Objekten und zum Ausführen von ABAP Unit-Tests.
   Lege noch keine Objekte an, ändere nichts und aktiviere nichts.
   ```
7. Vergleiche die Antwort mit der Liste aktivierter Tools. Eine Beschreibung allein belegt noch keine Backend-Verbindung. Bitte Copilot, die vorhandenen Strukturen `/DMO/TRAVEL_DATA` und `/DMO/BOOKING_DATA` über den verbundenen ADT-Workspace zu lesen und ihre Schlüssel und Felder zusammenzufassen, ohne sie zu bearbeiten.
8. Prüfe angeforderte Leseoperationen und ihre Systemdestination. Stelle sicher, dass tatsächlich Objektinhalte zurückgegeben werden. Falls der Zugriff fehlschlägt, verbinde die Destination erneut und prüfe vor dem Fortfahren, ob das Flight Reference Scenario installiert ist.

   > ✅ **Erfolg:** Copilot kann vorhandenen ABAP-Quellcode lesen; Tools für Anlage, Aktivierung und Tests sind verfügbar. Falls deine Version einen benötigten Objekttyp nicht per Tool anlegen kann, verwende in Übung 2 **ABAP: Create New ABAP Object** und lass Copilot anschließend den Quellcode bearbeiten.

</details>

---

## Zusammenfassung und nächste Übung
[↑ Zum Seitenanfang](#)

Du hast:

- Den ADT MCP Server in den Einstellungen der VS-Code-Erweiterung aktiviert.
- Über eine Benachrichtigung oder `MCP: List Servers` geprüft, ob der Server läuft.
- Bestätigt, dass die ADT-MCP-Tools für **`abap-developer`** in GitHub Copilot Chat sichtbar und aufrufbar sind.

Weiter geht es mit **[Übung 2: Die ABAP-Travel-Anwendung erzeugen](../ex02/README.de.md)**.

---
