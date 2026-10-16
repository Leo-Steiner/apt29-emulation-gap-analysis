
# Run 2 – Vorher/Nachher-Vergleich

**Zeitfenster Run 2:** 2026-10-16 18:19:44 – 18:59:55 UTC
**Agent:** vm-target

## Implementierte Fixes

| Fix | Ziel | ATT&CK ID | Umsetzung |
|---|---|---|---|
| Fix 1 | Discovery-Befehle | T1033, T1082, T1016 | Custom Wazuh-Regel 100200 |
| Fix 2 | LSASS ProcessAccess | T1003.001 | Sysmon ProcessAccess + ProcessAccessConfig |

## Vorher/Nachher-Tabelle

| Fähigkeit | ATT&CK ID | Run 2 (vor Fix) | Nach Fix | Verbesserung |
|---|---|---|---|---|
| OS Credential Dumping: LSASS | T1003.001 | Missed | Detected | ✅ |
| System Owner/User Discovery | T1033 | Missed | Detected | ✅ |
| System Information Discovery | T1082 | Missed | Detected | ✅ |
| System Network Configuration Discovery | T1016 | Missed | Detected | ✅ |
