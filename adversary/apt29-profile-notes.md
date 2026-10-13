# APT29 Adversary Profile Notes

## Gruppendaten

| Feld | Wert |
|---|---|
| Name | APT29 |
| Aliase | Cozy Bear, Midnight Blizzard, NOBELIUM, The Dukes, UNC2452 |
| ATT&CK ID | G0016 |
| Herkunft | Russland (SWR) |
| Ziele | Regierungen, Diplomatie, Think Tanks, Gesundheitswesen, Technologie |

## Ausgewählte TTPs (Phase 1 Entwurf)

| Tactic | Technique | ATT&CK ID |
|---|---|---|
| Initial Access / Execution | User Execution: Malicious File | T1204.002 |
| Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |
| Persistence | Registry Run Keys / Startup Folder | T1547.001 |
| Persistence | Scheduled Task | T1053.005 |
| Privilege Escalation / Defense Evasion | Process Injection | T1055 |
| Defense Evasion | Impair Defenses: Disable or Modify Tools | T1562.001 |
| Credential Access | OS Credential Dumping: LSASS Memory | T1003.001 |
| Credential Access | Unsecured Credentials | T1552 |
| Discovery | System Information Discovery | T1082 |
| Discovery | Account Discovery | T1087 |
| Discovery | System Network Configuration Discovery | T1016 |
| Lateral Movement | Remote Services | T1021 |
| Collection | Archive Collected Data | T1560 |
| Command and Control | Application Layer Protocol: Web Protocols | T1071.001 |
| Exfiltration | Exfiltration Over C2 Channel | T1041 |

## Quellen

- MITRE ATT&CK APT29 Group Page (G0016)
- CTID Adversary Emulation Library – APT29 Plan
- MITRE Caldera + Emu Plugin
