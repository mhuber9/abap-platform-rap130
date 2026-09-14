[English](prompt-guidelines.md) | **Deutsch**

# Prompt-Leitlinien für den ABAP-Travel-Workshop

Verwende diese Prompts mit **`abap-developer`** in **GitHub Copilot Chat**, verbunden mit den **ADT-MCP-Tools**. Verwende das Paket `$TMP` und ersetze `####` vor dem Absenden in Klassen- und Tabellennamen durch dein vierstelliges Teilnehmersuffix. Die verlinkten Übungen enthalten die vollständigen Vorgaben. Nutze sie für die erstmalige Erstellung, statt Copilot die Anwendungsschnittstellen selbst festlegen zu lassen.

## Allgemeine Grundsätze

- Gib exakte Objektnamen, das Paket `$TMP` und die verbundene Destination an.
- Verlasse dich auf die konfigurierten General- und Testing-Anweisungen in `abap-developer`. Wiederhole Regeln zu Cloud-Syntax, MCP-Nutzung, virtueller Workspace-Suche, Editorbedienung oder routinemäßigen Tests nicht in jedem Prompt.
- Beschreibe das Anwendungsverhalten, relevante Objekte und szenariospezifische Testfälle.
- Prüfe Quellcode vor Aktivierung und Tests. Prüfe vorgeschlagene Korrekturen, statt fehlgeschlagene Assertions abzuschwächen.
- Gestalte das Laden der Beispieldaten wiederholbar und greife ausschließlich lesend auf `/DMO/`-Daten zu. Die ausführbare Klasse steuert die Transaktionsgrenzen.
- Unterscheide beobachtete Tool-Ergebnisse von vorgeschlagenem Code oder nicht ausgeführten Prüfungen.

## Beispiel-Prompts nach Übung

### Übung 1 – Tools erkunden und Quellcode lesen

```text
Ermittle die verfügbaren ADT-Tools zum Anlegen von Klassen und Datenbanktabellen,
zum Aktivieren von Objekten und zum Ausführen von Unit-Tests. Lies /DMO/TRAVEL_DATA
und /DMO/BOOKING_DATA und fasse ihre Schlüssel zusammen.
Ändere und aktiviere nichts.
```

### Übung 2 – Die Anwendung anlegen

Verwende den vollständigen [Erstellungs-Prompt aus Übung 2](../exercises/ex02/README.de.md#übung-22-die-travel-tabellen-und-klassen-erzeugen). Er definiert `YTRAVEL####`, `YBOOKING####`, `YCL_TRAVEL_SERVICE_####` und `YCL_TRAVEL_APP_####` einschließlich Methodensignaturen. Die Hilfsklasse folgt in Übung 3.

Nach der Quellcodeprüfung:

```text
Aktiviere zunächst YTRAVEL#### und YBOOKING####,
dann YCL_TRAVEL_SERVICE_#### und anschließend YCL_TRAVEL_APP_####.
Berichte die tatsächlichen Aktivierungsergebnisse.
```

### Übung 3 – Beispieldaten laden und anzeigen

Verwende den vollständigen [Prompt zum Laden der Beispieldaten](../exercises/ex03/README.de.md#übung-31-beispieldaten-für-reisen-und-buchungen-laden), einschließlich Auswahl der Quelldaten und Transaktionsbehandlung. Für eine ausschließlich lesende Prüfung:

```text
Prüfe load_demo_data in YCL_TRAVEL_SERVICE_#### und den Aufrufer in YCL_TRAVEL_APP_####.
Erkläre, warum ein erneuter Lauf vorhandene Daten überspringt, wie Buchungszeilen auf
ausgewählte Reisen beschränkt werden und wie ein fehlgeschlagener Ladevorgang
zurückgerollt wird. Ändere nichts.
```

### Übung 4 – Kundenvalidierung ergänzen

```text
Verwende in YCL_TRAVEL_SERVICE_####->save_travel vor jedem Datenbankschreibzugriff
YCL_TRAVEL_HELPER_####->validate_customer.
Lehne initiale oder nicht vorhandene Kunden mit ty_result-success = abap_false
und einer aussagekräftigen Meldung ab. Behalte die Signatur bei und belasse die
Transaktionssteuerung im Aufrufer. Zeige Änderungen vor der Aktivierung zur Prüfung.
```

Verwende den vollständigen [Prompt zur Konsolenvalidierung](../exercises/ex04/README.de.md#übung-42-die-erweiterte-travel-anwendung-ausführen-und-testen), um gültige und ungültige Speicherversuche zu demonstrieren und die gespeicherte Zeile vor dem Rollback zu vergleichen.

### Übung 5 – Tests erzeugen und ausführen

```text
Erzeuge isolierte ABAP Unit-Tests für YCL_TRAVEL_HELPER_####->validate_customer
mit SQL-Doubles für /DMO/CUSTOMER. Decke vorhandene, fehlende und initiale IDs ab.
Zeige die Tests vor Aktivierung und Ausführung zur Prüfung.
```

Verwende die [Servicetestfälle](../exercises/ex05/README.de.md#übung-52-ablehnung-vor-dem-datenbankschreibzugriff-prüfen), um nachzuweisen, dass ungültige Speicherversuche keine Teilnehmerzeilen einfügen oder ändern. Nach der Prüfung beider Klassen:

```text
Führe ABAP Unit-Tests für YCL_TRAVEL_HELPER_####
und YCL_TRAVEL_SERVICE_#### aus. Berichte die tatsächlichen Ergebnisse.
Erkläre Fehler und schlage Korrekturen zur Prüfung vor, ohne die fehlgeschlagenen
Anforderungen zu entfernen.
```

### Übung 6 – Das Debugging vorbereiten

```text
Lies die Konsolenanwendung und identifiziere Stellen für Breakpoints in save_travel
und validate_customer, um die Versuche mit gültigem und ungültigem Kunden zu verfolgen.
Erkläre den erwarteten Call Stack. Ändere den Code nicht.
```

### Übung 7 – Den Kontext des eigenen Agenten prüfen

```text
Fasse mein Paket, meine Objektnamen, den Einstiegspunkt und die Transaktionsregeln zusammen.
Untersuche save_travel und erkläre, wie ungültige Kunden-IDs abgelehnt werden.
Ändere und aktiviere nichts.
```

## Tipps für bessere Ergebnisse

| Tipp | Nutzen |
|-----|-------------|
| Gib den tatsächlichen Aktivierungs- oder Testfehler an | Die Korrektur basiert auf beobachtetem Verhalten |
| Behalte die Methodenschnittstellen aus Übung 2 bei | Prompts und spätere Übungen bleiben kompatibel |
| Prüfe im Backend, ob Demo-Kunden fehlen | Vermeidet die Annahme, dass eine fest vorgegebene ID ungültig ist |
| Verwende Test-Doubles für Kunden-IDs in Unit-Tests | Tests bleiben unabhängig von gemeinsam genutzten Daten |
| Prüfe gespeicherte Zeilen vor Rollback oder Aufräumen | Erkennt ungültige Schreibzugriffe, die sonst verdeckt würden |
