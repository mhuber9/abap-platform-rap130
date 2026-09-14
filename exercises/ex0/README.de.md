[English](README.md) | **Deutsch**

[Startseite – Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln](../../README.de.md)

# Erste Schritte

## Einführung

Willkommen bei **RAP130**! Bevor du deine ABAP-Travel-Konsolenanwendung entwickelst, richtest du deine Entwicklungsumgebung in **Visual Studio Code** ein und verbindest sie mit deinem ABAP-System.

In dieser Übung installierst du Visual Studio Code und die ADT-Erweiterung und stellst eine Verbindung zu deinem ABAP-System her.

### Übungen

- [0.1 – Deine Gruppen-ID festlegen](#übung-01-deine-gruppen-id-festlegen)
- [0.2 – Visual Studio Code installieren](#übung-02-visual-studio-code-installieren)
- [0.3 – Die ADT-Erweiterung für Visual Studio Code installieren](#übung-03-die-adt-erweiterung-für-visual-studio-code-installieren)
- [0.4 – Visual Studio Code mit deinem ABAP-System verbinden](#übung-04-visual-studio-code-mit-deinem-abap-system-verbinden)
- [0.5 – Die Oberfläche von Visual Studio Code für ABAP kennenlernen](#übung-05-die-oberfläche-von-visual-studio-code-für-abap-kennenlernen)
- [Zusammenfassung](#zusammenfassung)

> ℹ️ **Erinnerung:** In Abschnitt 0.1 legst du eine **Gruppen-ID** fest. `####` steht in Klassen- und Tabellennamen für deine vierstellige Gruppen-ID. Alle Workshop-Objekte gehören zum vorhandenen Paket `$TMP`.

---

## Übung 0.1: Deine Gruppen-ID festlegen
[↑ Zum Seitenanfang](#)

> Lege eine Gruppen-ID fest, um deine Repository-Objekte im gesamten Workshop eindeutig zu kennzeichnen und Konflikte mit anderen Teilnehmern im selben System zu vermeiden.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

Da mehrere Teilnehmer dieselbe ABAP-Umgebung verwenden, folgt jedes angelegte Objekt einem Namensschema mit einem persönlichen Suffix.

Klassen- und Tabellennamen verwenden **`####`** für deine vierstellige Gruppen-ID. Lege alle Workshop-Objekte im vorhandenen lokalen Paket **`$TMP`** an. Du musst kein Paket erstellen.

Die Gruppen-ID muss **genau vier Ziffern** enthalten, zum Beispiel `0123`, `1042` oder `2026`. Behalte führende Nullen in jedem Objektnamen bei.

**Prüfe, ob eine Gruppen-ID bereits verwendet wird:**

1. Sobald Visual Studio Code verbunden ist (siehe Übung 0.4), öffne die **Command Palette** mit **`Ctrl+Shift+P`** (macOS: **`Cmd+Shift+P`**).
2. Gib `>ABAP: Open Object` ein und drücke **Enter**.
3. Suche nach **`YCL_TRAVEL*####`** und ersetze `####` durch dein gewähltes Suffix. Werden Ergebnisse angezeigt, ist die Gruppen-ID bereits belegt. Wähle eine andere.
4. Suche zusätzlich nach `YTRAVEL####`, `YBOOKING####` und `YCL_TRAVEL*####`. Verwende das Suffix nur, wenn keines dieser Workshop-Objekte existiert. Notiere es und verwende es in allen Übungen einheitlich.

> ⚠️ Wir empfehlen, die Gruppen-ID **`0000`** nicht zu verwenden. Wähle eine vierstellige Zahl, die nur du verwendest.

> ⚠️ Bei SAP-Workshops erhältst du deine Gruppen-ID **`####`** von der Kursleitung.

</details>

---

## Übung 0.2: Visual Studio Code installieren
[↑ Zum Seitenanfang](#)

> Lade Visual Studio Code herunter und installiere es, falls du dies noch nicht getan hast.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

1. Öffne einen Browser und rufe [https://code.visualstudio.com/](https://code.visualstudio.com/) auf.
2. Lade das Installationsprogramm für dein Betriebssystem herunter (Windows, macOS oder Linux).
3. Führe es aus und folge den Anweisungen auf dem Bildschirm.
4. Starte **Visual Studio Code** nach der Installation.

   > ℹ️ **Tipp:** Verwende eine aktuelle stabile Version von Visual Studio Code. Unter **Help > About** kannst du deine Version prüfen.

</details>

---

## Übung 0.3: Die ADT-Erweiterung für Visual Studio Code installieren
[↑ Zum Seitenanfang](#)

> Installiere die Erweiterung **ABAP Development Tools (ADT)** aus dem Visual Studio Marketplace. Sie verbindet Visual Studio Code mit deinem ABAP-Backend und enthält den integrierten **ADT MCP Server**, den du in Übung 1 verwendest.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

### Schritt 1: Aus dem Visual Studio Marketplace installieren

1. Öffne **Visual Studio Code**.
2. Öffne die Ansicht **Extensions**:
   - Drücke **`Ctrl+Shift+X`** (macOS: **`Cmd+Shift+X`**) oder
   - klicke auf das Symbol **Extensions** in der Activity Bar am linken Rand.
3. Gib Folgendes in die Suchleiste ein:
   ```
   ABAP Development Tools
   ```
4. Suche die von **SAP** veröffentlichte Erweiterung **"ABAP Development Tools"** und klicke auf **Install**.
5. Warte, bis die Installation abgeschlossen ist.

   > ✅ **Erfolg:** Unter den Erweiterungen mit Status **Installed** sollte „ABAP Development Tools“ erscheinen.

   ![ADT-Erweiterung im Visual Studio Marketplace](images/ex0_adt_marketplace.png)

### Schritt 2: Die Installation prüfen

1. Öffne die **Command Palette** (**`Ctrl+Shift+P`**) und gib `ABAP` ein. Du solltest unter anderem diese ABAP-Befehle sehen:
   - `ABAP: New Destination...`
   - `ABAP: Open Object...`
   - `ABAP: Create New ABAP Object...`
   - `ABAP: Activate`

   > ✅ Wenn diese Befehle angezeigt werden, ist die Erweiterung korrekt installiert.

   ![Installierte ADT-Erweiterung mit ABAP-Befehlen in der Command Palette](images/ex0_adt_installed.png)

</details>

---

Installiere vor dem Fortfahren **GitHub Copilot** über die Ansicht Extensions, melde dich mit deinem GitHub-Konto an und prüfe, ob Copilot Chat für dein Konto verfügbar ist. Wähle **`abap-developer`** in der Agentenauswahl. Falls der Agent fehlt, frage die Kursleitung nach seiner Konfiguration.

## Übung 0.4: Visual Studio Code mit deinem ABAP-System verbinden
[↑ Zum Seitenanfang](#)

> Lege in Visual Studio Code eine **Destination** an, um dich mit deinem ABAP-System zu verbinden, und füge sie dem Workspace hinzu.
>
> Wähle den zu deinem System passenden Verbindungstyp:
> - **HTTP** – SAP BTP ABAP Environment oder SAP S/4HANA Cloud Public Edition
> - **RFC** – SAP S/4HANA On-Premise oder SAP S/4HANA Cloud Private Edition (SAP Logon muss auf deinem Rechner eingerichtet sein)

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

### Option A: HTTP-Destination für Cloud-Systeme

1. Öffne die **Command Palette** (**`Ctrl+Shift+P`**).
2. Gib `ABAP: New Destination` ein und wähle **"ABAP: New Destination..."**.
3. Wähle **HTTP** als Verbindungstyp.
4. Gib die **System-URL** deines ABAP-Cloud-Systems ein. Das URL-Format lautet:
   ```
   https://<system-id>.abap.<region>.hana.ondemand.com
   ```
5. Drücke **Enter** und gib eine kurze **ID** für die Destination ein, zum Beispiel `BTP_DEV`.
6. Drücke **Enter**. Die Verbindung wird hergestellt.

   ```
   Command Palette → "ABAP: New Destination..."
     ↓
   Connection Type → HTTP
     ↓
   System URL → https://my-system.abap.eu10.hana.ondemand.com
     ↓
   Destination ID → BTP_DEV
     ↓
   ✅ Verbindung hergestellt!
   ```

---

### Option B: RFC-Destination für On-Premise-Systeme

> ℹ️ **Voraussetzung:** Dein System muss vor dem Fortfahren in **SAP Logon** (SAP GUI) mit RFC-Konnektivität eingerichtet sein.

1. Öffne die **Command Palette** (**`Ctrl+Shift+P`**).
2. Gib `ABAP: New Destination` ein und wähle **"ABAP: New Destination..."**.
3. Wähle **RFC** als Verbindungstyp.
4. Eine Liste der Systeme aus deiner **SAP-Logon**-Konfiguration wird angezeigt. Wähle dein Zielsystem.
5. Gib deinen **Benutzernamen** ein und drücke **Enter**.
6. Gib die **Mandantennummer** ein, zum Beispiel `100`, und drücke **Enter**.
7. Gib deine **Anmeldesprache** ein, zum Beispiel `EN`, und drücke **Enter**.
8. Die Verbindung wird hergestellt.

   ```
   Command Palette → "ABAP: New Destination..."
     ↓
   Connection Type → RFC
     ↓
   System → S4D - Development System
     ↓
   Username → DEVELOPER01
     ↓
   Client → 100
     ↓
   Language → EN
     ↓
   ✅ Verbindung hergestellt!
   ```

---

### Die Destination zum Workspace hinzufügen

> ⚠️ **Wichtig:** Dieser Schritt ist erforderlich, um die ADT-Erweiterung zu aktivieren und den lokalen MCP-Server in Übung 1 zu starten.

1. Öffne die **Command Palette** (**`Ctrl+Shift+P`**).
2. Gib `ABAP: Add Destination as Folder` ein und wähle **"ABAP: Add Destination as Folder to the Workspace..."**.
3. Wähle deine neu angelegte Destination.
4. Bei **HTTP**-Systemen öffnet sich ein Browserfenster. Melde dich mit den Zugangsdaten deines ABAP-Systems an. Bei **RFC**-Systemen wird die Verbindung unmittelbar mit deinen SAP-Logon-Zugangsdaten hergestellt.
5. Nach der Anmeldung erscheint die Systemverbindung als **Ordner** in der Ansicht Explorer auf der linken Seite von Visual Studio Code.

</details>

---

## Übung 0.5: Die Oberfläche von Visual Studio Code für ABAP kennenlernen
[↑ Zum Seitenanfang](#)

> Mache dich mit den wichtigsten Bereichen von Visual Studio Code vertraut, die du im Workshop verwendest.

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

### Wichtige Bereiche für die ABAP-Entwicklung

| Bereich | Position | Zweck |
|------|----------|---------|
| **Activity Bar** | Ganz links | Zwischen Explorer, Search, Source Control, Run & Debug und Extensions wechseln |
| **Explorer (Workspace)** | Linke Seitenleiste | Durch Pakete, Objekttypen und Objekte im ABAP-System navigieren |
| **Editor** | Mitte | ABAP-Quellcode bearbeiten: Klassen und Datenbanktabellendefinitionen |
| **Coding agent chat** (z. B. GitHub Copilot) | Rechte Seitenleiste oder Panel | KI-gestützter Chat für Prompts und MCP-Tool-Aufrufe |
| **Problems Panel** | Unten | Syntaxfehler und Warnungen |
| **ABAP Console** | Unten | Ausgabe der Travel-Anwendung |
| **Terminal** | Unten | Integrierte Shell |
| **Status Bar** | Ganz unten | Aktuelle Zeile/Spalte, Sprache und verbundenes System |

### Wichtige Tastenkombinationen

| Aktion | Windows/Linux | macOS |
|--------|---------------|-------|
| Command Palette | **`Ctrl+Shift+P`** | **`Cmd+Shift+P`** |
| Open ABAP Object | **`Ctrl+Shift+A`** | **`Cmd+Shift+A`** |
| Create ABAP Object | **`Ctrl+Shift+Alt+N`** | **`Cmd+Shift+Option+N`** |
| Activate object | **`Ctrl+F3`** | **`Cmd+F3`** |
| Activate all inactive | **`Ctrl+Shift+F3`** | **`Cmd+Shift+F3`** |
| Save | **`Ctrl+S`** | **`Cmd+S`** |
| Find/Replace | **`Ctrl+H`** | **`Cmd+H`** |
| Run ABAP Unit Tests | **`Ctrl+Shift+F10`** | **`Cmd+Shift+F10`** |

> ℹ️ **Tipp:** Ersetze mit **Find/Replace** (**`Ctrl+H`**) den Platzhalter `####` durch deine vierstellige Gruppen-ID. Lass den Paketnamen `$TMP` unverändert.

### Im ABAP-System navigieren

1. Klappe in der Ansicht **Explorer** den Ordner deiner Systemverbindung auf.
2. Navigiere durch die Pakethierarchie zu den Objekten.
3. Öffne mit **`Ctrl+Shift+A`** schnell ein Objekt anhand seines Namens. Du kannst Platzhalter wie `YCL_TRAVEL*####` verwenden.
4. Objekte mit noch nicht aktivierten Backend-Änderungen sind mit **(L)** markiert. Du musst sie **aktivieren** (**`Ctrl+F3`**), damit sie im System aktiv werden.

</details>

---

## Zusammenfassung
[↑ Zum Seitenanfang](#)

Du hast:

- Deine Gruppen-ID (`####`) für die Objektnamen festgelegt.
- Visual Studio Code installiert.
- Die ADT-Erweiterung einschließlich ADT MCP Server installiert.
- Eine HTTP-Destination angelegt und Visual Studio Code mit deinem ABAP-Cloud-System verbunden.
- Die Destination zum Workspace hinzugefügt und dich angemeldet.
- Die wichtigsten Bereiche von Visual Studio Code für ABAP kennengelernt.

Weiter geht es mit **[Übung 1: Den ADT MCP Server aktivieren](../ex01/README.de.md)**.

---
