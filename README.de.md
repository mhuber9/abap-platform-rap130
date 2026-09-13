[English](README.md) | **Deutsch**

# Eine ABAP-Travel-Anwendung mit GitHub Copilot und ADT in Visual Studio Code entwickeln

## Beschreibung

Dieses Repository enthält einen praktischen Workshop für ABAP-Entwickler, die **GitHub Copilot mit ADT in Visual Studio Code** kennenlernen möchten.

Du entwickelst eine Anwendung für Reisen und Buchungen mit **normalen ABAP-Cloud-Klassen und Datenbanktabellen**. Eine ausführbare Klasse zeigt Daten und Speicherergebnisse in der **ABAP Console** an. Copilot und der **ADT MCP Server** unterstützen dich beim Anlegen von Objekten, Erweitern des Codes und Erzeugen von Unit-Tests.

Der Repository-Name behält `RAP130` aus Gründen der Kontinuität bei. Die Übungen verwenden weder eine RAP-Laufzeit noch eine Fiori-Oberfläche.

**Inhaltsverzeichnis**

- [Voraussetzungen](#voraussetzungen)
- [Überblick](#überblick)
- [Übungen](#übungen)
- [Bekannte Probleme](#bekannte-probleme)
- [Unterstützung](#unterstützung)
- [Weitere Informationen](#weitere-informationen)

## Voraussetzungen

- Kenntnisse in ABAP-Klassen, grundlegendem SQL und ABAP-Entwicklung; im Mittelpunkt steht der Copilot-Workflow.
- [Visual Studio Code](https://code.visualstudio.com/) und [SAP ADT for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=SAPSE.adt-vscode), einschließlich ADT MCP Server.
- GitHub Copilot Chat mit verfügbarem Agenten **`abap-developer`**, angemeldet und mit Zugriff auf den virtuellen ADT-Workspace. Falls der Agent fehlt, frage die Kursleitung nach seiner Konfiguration.
- Ein Entwicklungssystem auf **SAP BTP ABAP Environment**, **SAP S/4HANA Cloud Public Edition** oder einer geeigneten **SAP S/4HANA Cloud Private Edition**, das die installierte ADT-Erweiterung und deren MCP-Funktionen unterstützt. Kläre die Backend- und Versionsanforderungen anhand der verlinkten SAP-Dokumentation mit der Kursleitung.
- Das installierte [ABAP Flight Reference Scenario](https://github.com/SAP-samples/abap-platform-refscen-flight) mit `/DMO/TRAVEL_DATA`, `/DMO/BOOKING_DATA` sowie befüllten Quelltabellen `/DMO/TRAVEL`, `/DMO/BOOKING` und `/DMO/CUSTOMER`, auf die der Workshop-Code zugreifen kann.
- Berechtigungen zum Anlegen und Aktivieren der Teilnehmerobjekte, Ausführen von Konsolenklassen und ABAP Unit-Tests sowie optional zum Debuggen. Das ABAP SQL Test Double Framework muss verfügbar sein.
- Zugriff auf das vorhandene lokale Paket `$TMP` zum Anlegen der Workshop-Objekte. Ein neues Paket oder eine Eclipse-Einrichtung ist für diese Übungen nicht erforderlich.

Lass die Kursleitung bestätigen, welche Backend-Freischaltungen für die ADT-Agentenwerkzeuge erforderlich sind. Die Nutzung von Copilot ersetzt diese Systemvoraussetzungen nicht.

> Bei SAP-Veranstaltungen stellt die Kursleitung die Systemzugangsdaten und Gruppen-IDs bereit.

## Überblick

<details>
  <summary>🔵 Zum Aufklappen klicken!</summary>

Der Workshop führt von der Einrichtung der Umgebung bis zu einer funktionierenden Anwendung mit Validierung und Tests:

- Verbinde VS Code mit deinem ABAP-System und aktiviere den ADT MCP Server.
- Untersuche mit Copilot die Referenzdefinitionen und lege Tabellen und Klassen für Reisen und Buchungen an.
- Prüfe den erzeugten Quellcode und aktiviere die voneinander abhängigen Objekte.
- Lade einen kleinen Beispieldatensatz in deine Teilnehmertabellen und zeige Reisen mit ihren Buchungen an.
- Ergänze eine Kundenvalidierung vor dem Speichern und prüfe erfolgreiche sowie abgelehnte Speichervorgänge.
- Erzeuge isolierte Unit-Tests und untersuche Fehler und Korrekturen.
- Debugge optional die ausführbare Anwendung und erstelle einen eigenen Agenten.

Die ausführbare Klasse `YCL_TRAVEL_APP_####` ruft `YCL_TRAVEL_SERVICE_####` auf. Der Service liest und speichert Teilnehmerdaten und verwendet `YCL_TRAVEL_HELPER_####` zur Kundenvalidierung. Die ausführbare aufrufende Klasse steuert Commit und Rollback. Das Laden der Beispieldaten erhält vorhandene Teilnehmerdaten; `/DMO/`-Daten werden ausschließlich gelesen.

</details>

## Übungen

Bearbeite die Pflichtübungen der Reihe nach. Verwende das Paket `$TMP` und ersetze `####` in Klassen- und Tabellennamen durch dein vierstelliges Teilnehmersuffix. Die [Prompt-Leitlinien](resources/prompt-guidelines.de.md) enthalten wiederverwendbare Prompts und Links zu den vollständigen Vorgaben.

VS-Code-Befehle, Bezeichnungen der Oberfläche, ABAP-Bezeichner und Beispielausgaben bleiben in dieser deutschen Fassung auf Englisch.

| Übung | Lernschwerpunkt |
|----------|----------------|
| [Erste Schritte](exercises/ex0/README.de.md) | Umgebung und Systemverbindung |
| [Übung 1: Den ADT MCP Server aktivieren](exercises/ex01/README.de.md) | Werkzeuge erkunden und auf Quellcode zugreifen |
| [Übung 2: Die ABAP-Travel-Anwendung erzeugen](exercises/ex02/README.de.md) | Tabellen, Klassen, Prüfung und Aktivierung |
| [Übung 3: Die Travel-Anwendung ausführen](exercises/ex03/README.de.md) | Beispieldaten, Konsolenausführung und Hilfsklasse |
| [Übung 4: Eine Validierung ergänzen](exercises/ex04/README.de.md) | Validierung vor dem Datenbankschreibzugriff |
| [Übung 5: ABAP Unit-Tests erzeugen](exercises/ex05/README.de.md) | Isolierte Tests und Fehlerkorrektur |

### Optionale Übungen

| Übung | Lernschwerpunkt |
|----------|----------------|
| [Übung 6: ABAP-Code in Visual Studio Code debuggen](exercises/ex06/README.de.md) | Breakpoints, Einzelschritte, Watches und Aufrufliste |
| [Übung 7: Einen eigenen Agenten erstellen](exercises/ex07/README.de.md) | Wiederverwendbarer Anwendungskontext und Anweisungen |

## Bekannte Probleme

- ADT-Befehlsnamen und verfügbare MCP-Werkzeuge können sich je nach installierter Version unterscheiden. Ermittle die tatsächlich verfügbaren Werkzeuge in Übung 1 und verwende bei Bedarf die beschriebene manuelle Objektanlage.
- Ohne Quelldaten im Flight Reference Scenario können keine Beispieldaten geladen werden. Bitte die Kursleitung, die Daten vor dem Fortfahren bereitzustellen.
- Der überarbeitete Ablauf wurde lokal auf Konsistenz der Dokumentation geprüft. Backend-Aktivierung, Konsolenausführung, ABAP Unit-Tests und Debugger-Verbindung müssen noch auf einem verbundenen Workshop-System geprüft werden.

## Unterstützung

[Erstelle ein Issue](https://github.com/mhuber9/abap-platform-rap130/issues), wenn du Probleme mit diesem angepassten Workshop findest. Gib die Übung, ADT- und Backend-Versionen sowie die tatsächliche Fehlermeldung an.

Nutze für allgemeine ABAP-Fragen die [SAP Community](https://community.sap.com/).

## Weitere Informationen

- [Agentic AI for ABAP Development](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/agentic-ai-development?locale=en-US)
- [ADT MCP Tools](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/mcp-tools?locale=en-US)
- [ABAP Basic Features for Visual Studio Code](https://developers.sap.com/tutorials/abap-environment-adt-coretools-vscode)
- [Dokumentation zu ADT for Visual Studio Code](https://help.sap.com/docs/abap-cloud/abap-development-tools-for-visual-studio-code/abap-development-tools-for-visual-studio-code?locale=en-US)
- [Eigene Agenten in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)

## Mitwirken

Wenn du Code, Korrekturen oder Verbesserungen beitragen möchtest, erstelle einen Pull Request. Aus rechtlichen Gründen wirst du beim ersten Pull Request für dieses Projekt aufgefordert, ein DCO zu akzeptieren. Dies geschieht automatisch während der Einreichung. SAP verwendet den [Standard-DCO-Text der Linux Foundation](https://developercertificate.org/).

## Lizenz

Copyright (c) 2026 SAP SE oder ein SAP-Konzernunternehmen und die Mitwirkenden an abap-platform-rap130. Alle Rechte vorbehalten. Dieses Projekt steht unter der Apache Software License, Version 2.0, sofern in der [LICENSE](LICENSES/Apache-2.0.txt) nichts anderes angegeben ist. Du darfst Dateien dieses Projekts nur im Einklang mit dieser Lizenz verwenden.

Soweit nicht gesetzlich vorgeschrieben oder schriftlich vereinbart, wird die unter der Lizenz verteilte Software ohne Gewährleistung oder Bedingungen jeglicher Art bereitgestellt, weder ausdrücklich noch stillschweigend. Die maßgeblichen Bestimmungen zu Rechten und Einschränkungen findest du im englischen Lizenztext.
