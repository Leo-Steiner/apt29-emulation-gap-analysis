# Defense Recommendations – APT29 Emulation Lab

**Basis:** Gap-Analyse aus Run 2 (2026-10-16, 18:19–18:59 UTC)
**Detection Coverage vor Fixes:** 0 % (0 von 30 Fähigkeiten)
**Detection Coverage nach Fixes:** 30 % (9 von 30 Fähigkeiten)

---

## 1. Technische Empfehlungen

### 1.1 Detection Engineering (umgesetzt)

Die folgenden Wazuh-Regeln wurden im Rahmen der Gap-Analyse implementiert und haben die Detection Coverage von 0 % auf 30 % gesteigert (12 Custom Rules über 5 Fixes):

| Rule ID | Ziel | ATT&CK ID | Status |
|---|---|---|---|
| 100200 | Discovery-Befehle (whoami, hostname, ipconfig, systeminfo) | T1033, T1082, T1016 | ✅ implementiert |
| 100202–100204 | Scheduled Task Manipulation | T1053.005 | ✅ implementiert |
| 100205–100208 | Windows Service Persistence | T1543.003 | ✅ implementiert |
| 100210–100213 | Collection (Compress-Archive, rar.exe, Get-Clipboard, Get-Keystrokes) | T1560.001, T1115, T1056.001 | ✅ implementiert |
| Sysmon ProcessAccess | LSASS-Zugriff (Event ID 10) | T1003.001 | ✅ implementiert |

### 1.2 Detection Engineering (empfohlen, nicht umgesetzt)

Für die verbleibenden Gaps werden folgende Regeln empfohlen:

| Ziel | ATT&CK ID | Empfohlene Maßnahme |
|---|---|---|
| Process Discovery | T1057 | Wazuh-Regel für `tasklist.exe` und `Get-Process` |
| Permissions Groups Discovery | T1069 | Wazuh-Regel für `net localgroup administrators` |
| Account Discovery | T1087 | Wazuh-Regel für `net user /domain` |
| System Network Connections Discovery | T1049 | Wazuh-Regel für `netstat -ano` |
| Credentials In Files – Chrome | T1555.003 | Wazuh-Regel für `accesschk.exe` auf Browser-Profilen |
| Credentials In Files – Private Keys | T1552.004 | Wazuh-Regel für `Import-PfxCertificate` und `.pfx`-Dateizugriff |
| RTLO-Masquerading | T1036 | Wazuh-Regel für Dateinamen mit `U+202E` (Right-to-Left Override) |
| UAC-Bypass | T1548.002 | Wazuh-Regel für Registry-Änderungen unter `HKCU:\Software\Classes\Folder` |
| Process Injection | T1055 | Sysmon Event ID 8 mit `GrantedAccess`-Filter verfeinern |
| Screen Capture | T1113 | Wazuh-Regel für `Get-Screenshot` und ähnliche Cmdlets |

### 1.3 Telemetrie-Hardening

| Maßnahme | Nutzen | ATT&CK-Bezug |
|---|---|---|
| Sysmon-Clipboard-Monitoring aktivieren (Event ID 24) | Erkennt Clipboard-Zugriffe | T1115 |
| Sysmon-ImageLoad aktivieren (Event ID 7) | Erkennt DLL-Injection und Side-Loading | T1055, T1574 |
| Windows-Audit-Policy für „Kernel Object" aktivieren | Erkennt LSASS-Zugriffe auf OS-Ebene | T1003.001 |
| Windows-Audit-Policy für „Process Creation" mit Kommandozeile | Ergänzt Sysmon Event ID 1 | T1059 |
| PowerShell Script Block Logging aktivieren | Erkennt obfuskierte PowerShell | T1059.001 |
| PowerShell Module Logging aktivieren | Erkennt geladene Module | T1059.001 |
| Windows Defender AMSI-Logging aktivieren | Erkennt Script-basierte Angriffe | T1059 |

### 1.4 Preventive Controls

| Kontrolle | Ziel | ATT&CK-Bezug |
|---|---|---|
| Application Allow-Listing (AppLocker / WDAC) | Verhindert Ausführung nicht genehmigter Binaries | T1204, T1059 |
| PowerShell Constrained Language Mode | Verhindert obfuskierte PowerShell-Skripte | T1059.001 |
| Credential Guard aktivieren | Schützt LSASS vor Dumping | T1003.001 |
| LSA Protection (RunAsPPL) aktivieren | Verhindert LSASS-Zugriff durch Nicht-System-Prozesse | T1003.001 |
| Least Privilege für Dienstkonten | Reduziert Persistence-Möglichkeiten | T1543.003 |
| Netzwerksegmentierung | Verhindert laterale Bewegung | T1021 |
| MFA für alle Remote-Zugriffe | Verhindert Credential-Missbrauch | T1078, T1021 |
| Makro-Blockierung in Office | Verhindert Phishing-Payloads | T1204.002 |

---

## 2. Nicht-technische Empfehlungen

### 2.1 Security Awareness und Schulung

| Maßnahme | Zielgruppe | Häufigkeit | ATT&CK-Bezug |
|---|---|---|---|
| Phishing-Simulationen | Alle Mitarbeiter | Quartalsweise | T1204.002 (Initial Access) |
| Schulung zu Makro-Sicherheit | Office-Nutzer | Jährlich | T1204.002 |
| Schulung zu USB-Geräten | Alle Mitarbeiter | Jährlich | T1204.002 |
| Social-Engineering-Workshop | Führungskräfte | Jährlich | T1598 |

**Begründung:** APT29 verwendet häufig Phishing als Initial Access (T1204.002). Mitarbeiter, die Phishing-E-Mails erkennen, reduzieren das Risiko erheblich.

### 2.2 Incident Response und Prozesse

| Maßnahme | Beschreibung | ATT&CK-Bezug |
|---|---|---|
| IR-Playbooks erstellen | Dokumentierte Schritte für jede Angriffsphase | Alle |
| Tabletop-Übungen | Simulation eines APT-Angriffs mit Führungsteam | Alle |
| Eskalationsmatrix | Wer wird wann informiert | Alle |
| Forensische Bereitschaft | Tools und Prozesse für Incident-Untersuchung | T1003, T1055 |
| Kontinuierliches Monitoring | 24/7-Überwachung oder externer SOC | Alle |

**Begründung:** Ein Angriff wie APT29 erfordert eine koordinierte Reaktion. Ohne Playbooks und Übungen geht wertvolle Zeit verloren.

### 2.3 Policy und Governance

| Maßnahme | Beschreibung | ATT&CK-Bezug |
|---|---|---|
| Logging-Standards definieren | Welche Events müssen mindestens 90 Tage aufbewahrt werden | Alle |
| Patch-Management-Prozess | Monatliche Patches, kritische innerhalb 72 Stunden | T1190, T1203 |
| Access-Review-Prozess | Quartalsweise Überprüfung aller Berechtigungen | T1078, T1543 |
| Change-Management | Kontrollierte Änderungen an Sicherheitskonfigurationen | T1562 |
| Notfallplan | Was passiert bei einem bestätigten Angriff | Alle |

### 2.4 Lieferanten- und Drittanbieter-Management

| Maßnahme | Beschreibung | ATT&CK-Bezug |
|---|---|---|
| Lieferanten-Audits | Regelmäßige Sicherheitsüberprüfung kritischer Lieferanten | T1195, T1199 |
| Vertragliche Sicherheitsanforderungen | Mindestanforderungen an Sicherheit in Verträgen | T1195 |
| Monitoring von Drittanbieter-Zugriffen | Wer greift wann auf welche Systeme zu | T1078, T1199 |

**Begründung:** APT29 hat den SolarWinds-Supply-Chain-Angriff durchgeführt (T1195). Lieferanten-Management ist daher essenziell.

---

## 3. EU-Regulatorik und Compliance

### 3.1 NIS2-Richtlinie

Die NIS2-Richtlinie (EU 2022/2555) verpflichtet Betreiber kritischer Infrastrukturen zu:

| NIS2-Anforderung | Umsetzung im Kontext dieses Projekts |
|---|---|
| Risikomanagement | Regelmäßige Gap-Analysen und Penetrationstests |
| Incident-Meldung | Meldung schwerwiegender Vorfälle innerhalb 24 Stunden |
| Business Continuity | Notfallpläne und Wiederherstellungsstrategien |
| Lieferantensicherheit | Audits und vertragliche Anforderungen |
| Schulung | Regelmäßige Security-Awareness-Schulungen |

### 3.2 DSGVO

| DSGVO-Anforderung | Bezug zu APT29 |
|---|---|
| Art. 32 – Sicherheit der Verarbeitung | Technische Maßnahmen wie Verschlüsselung, Zugriffskontrolle, Detection |
| Art. 33 – Meldung von Datenschutzverletzungen | 72-Stunden-Meldefrist bei Datenpanne |
| Art. 25 – Datenschutz durch Technikgestaltung | Security by Design in allen Systemen |

### 3.3 Empfehlung für österreichische Organisationen

Österreichische Organisationen im öffentlichen Sektor oder in kritischen Infrastrukturen sollten:

1. Die NIS2-Anforderungen bis zur nationalen Umsetzung (NISG-Novelle) vorbereiten.
2. Die DSGVO-Meldekette mit der NIS2-Meldekette synchronisieren.
3. Regelmäßige APT-Emulationen als Teil des Risikomanagements durchführen.
4. Detection-Coverage als KPI in das Sicherheitsreporting aufnehmen.

---

## 4. Priorisierung der Empfehlungen

| Priorität | Empfehlung | Aufwand | Wirkung |
|---|---|---|---|
| **Hoch** | Detection Rules für verbleibende Gaps (T1057, T1069, T1087, T1049) | Gering | Hoch |
| **Hoch** | Credential Guard + LSA Protection aktivieren | Mittel | Sehr hoch |
| **Hoch** | PowerShell Script Block Logging aktivieren | Gering | Hoch |
| **Mittel** | Sysmon Clipboard + ImageLoad aktivieren | Gering | Mittel |
| **Mittel** | IR-Playbooks und Tabletop-Übungen | Mittel | Hoch |
| **Mittel** | Phishing-Simulationen | Mittel | Hoch |
| **Niedrig** | Lieferanten-Audits | Hoch | Mittel |
| **Niedrig** | NIS2-Vorbereitung | Hoch | Mittel |

---

## 5. Fazit

Die Gap-Analyse hat gezeigt, dass ein SIEM-gestütztes Detection-Setup (Wazuh + Sysmon) in der Lage ist, einen erheblichen Teil der APT29-Techniken zu erkennen, wenn die Regeln entsprechend konfiguriert sind. Die Detection Coverage konnte durch 5 gezielte Fixes von 0 % auf 30 % gesteigert werden.

Die größten verbleibenden Lücken liegen im Bereich Process Discovery (T1057) und Credential Access (T1555.003, T1552.004) sowie im Bereich der Defense Evasion (T1036). Hier sind weitere Custom Rules und Preventive Controls erforderlich.

Für einen umfassenden Schutz gegen APT29 sind technische Maßnahmen allein nicht ausreichend. Sie müssen durch organisatorische Prozesse, Schulungen und regulatorische Compliance ergänzt werden. Nur ein mehrschichtiger Ansatz (Defense in Depth) kann das Risiko eines erfolgreichen APT-Angriffs signifikant reduzieren.
