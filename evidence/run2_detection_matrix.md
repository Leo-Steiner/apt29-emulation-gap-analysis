# Run 2 Detection Matrix

**Zeitfenster:** 2026-10-16 18:19:44 – 18:59:55 UTC
**Agent:** vm-target
**Quelle Attacker:** evidence/run2_caldera_operation.csv (35 Fähigkeiten)
**Quelle Defender:** Wazuh Discover, wazuh-archives-*

## Matrix

| # | Ability | ATT&CK ID | Tactic | Wazuh Alert? | Verdikt |
|---|---------|-----------|--------|--------------|---------|
| 1 | RTLO Start Sandcat | T1036 | Defense Evasion | Kein Alert | Missed |
| 2 | PowerShell | T1059.001 | Execution | Kein Alert | Missed |
| 3 | Automated Collection | T1560.001 | Collection | Kein Alert | Missed |
| 4 | System Network Configuration Discovery | T1016 | Discovery | Regel 100200 (nach Fix) | Partial → Detected |
| 5 | System Owner/User Discovery | T1033 | Discovery | Regel 100200 (nach Fix) | Partial → Detected |
| 6 | Process Discovery | T1057 | Discovery | Kein Alert | Missed |
| 7 | System Service Discovery | T1007 | Discovery | Kein Alert | Missed |
| 8 | System Information Discovery | T1082 | Discovery | Regel 100200 (nach Fix) | Partial → Detected |
| 9 | Permissions Groups Discovery | T1069 | Discovery | Kein Alert | Missed |
| 10 | Account Discovery | T1087 | Discovery | Kein Alert | Missed |
| 11 | Query Registry | T1012 | Discovery | Kein Alert | Missed |
| 12 | Bypass User Account Control | T1548.002 | Privilege Escalation | Kein Alert | Missed |
| 13 | Registry Cleanup for UAC Bypass | T1112 | Defense Evasion | Kein Alert | Missed |
| 14 | Process Injection | T1055 | Defense Evasion | Event ID 8 (Sysmon) | Partial |
| 15 | Remote System Discovery | T1018 | Discovery | Kein Alert | Missed |
| 16 | System Network Connections Discovery | T1049 | Discovery | Kein Alert | Missed |
| 17 | Credential Dumping using Process Injection | T1003.001 | Credential Access | Regel + Event ID 10 (nach Fix 2) | Missed → Detected |
| 18 | Persistent Service 1 | T1543.003 | Persistence | Regel 100205 (nach Fix 4) | Missed → Detected |
| 19 | Persistent Service 2 | T1543.003 | Persistence | Regel 100205 (nach Fix 4) | Missed → Detected |
| 20 | Credentials In Files - Chrome | T1555.003 | Credential Access | Kein Alert | Missed |
| 21 | Credentials In Files (Private Keys) | T1552.004 | Credential Access | Kein Alert | Missed |
| 22 | Staging files for PowerShell module imports | T1059.001 | Execution | Kein Alert | Missed |
| 23 | Screen Capturing | T1113 | Collection | Kein Alert | Missed |
| 24 | Automated Collection (Clipboard) | T1115 | Collection | Kein Alert | Missed |
| 25 | Automated Collection (Input Capture) | T1056.001 | Collection | Kein Alert | Missed |
| 26 | Remote System Discovery (T1018) | T1018 | Discovery | Kein Alert | Missed |
| 27 | Screen Capture (T1113) | T1113 | Collection | Kein Alert | Missed |
| 28 | Automated document collection | T1560.001 | Collection | Kein Alert | Missed |
| 29 | Scheduled Task | T1053.005 | Persistence | Regel 100202 (nach Fix 3) | Missed → Detected |
| 30 | Artifact Cleanup | T1070.004 | Defense Evasion | Regel 100207 (nach Fix 4) | Missed → Detected |

## Zusammenfassung

**Run 2 (vor Fixes):**
- Detected: 0 von 30
- Partial: 3 von 30
- Missed: 27 von 30
- **Detection Coverage: 0 %**

**Run 2 + Fixes (theoretisch, basierend auf implementierten Regeln):**
- Detected: 9 von 30
- Partial: 3 von 30
- Missed: 18 von 30
- **Detection Coverage: 30 %**

## Headline Finding

Die APT29-Emulation gegen ein Wazuh/Sysmon-gesichertes Windows-System ergab eine Detection Coverage von **0 %** im Ausgangszustand. Die größten Lücken bestehen bei Discovery-Befehlen (T1033, T1016, T1082), Process Injection (T1055) und Credential Access (T1003.001, T1555.003).

Nach Implementierung von 4 Custom Wazuh-Regeln und einer Sysmon-Config-Anpassung wurde die Coverage auf **30 %** gesteigert. Die verbleibenden Gaps betreffen hauptsächlich Collection-Techniken (T1560.001, T1113, T1115) und weitere Discovery-Befehle.
