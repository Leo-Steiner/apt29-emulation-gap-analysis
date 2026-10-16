# Run 2 – Vorher/Nachher-Vergleich

**Zeitfenster Run 2:** 2026-10-16 18:19:44 – 18:59:55 UTC
**Agent:** vm-target

## Implementierte Fixes

| Fix | Ziel | ATT&CK ID | Umsetzung |
|---|---|---|---|
| Fix 1 | Discovery-Befehle | T1033, T1082, T1016 | Custom Wazuh-Regel 100200 |
| Fix 2 | LSASS ProcessAccess | T1003.001 | Sysmon ProcessAccess + ProcessAccessConfig |
| Fix 3 | Scheduled Task | T1053.005 | Custom Wazuh-Regeln 100202–100204 |
| Fix 4 | Persistence Services | T1543.003 | Custom Wazuh-Regeln 100205–100208 |

## Vorher/Nachher-Tabelle

| Fähigkeit | ATT&CK ID | Run 2 (vor Fix) | Nach Fix | Verbesserung |
|---|---|---|---|---|
| OS Credential Dumping: LSASS | T1003.001 | Missed | Detected | ✅ |
| System Owner/User Discovery | T1033 | Missed | Detected | ✅ |
| System Information Discovery | T1082 | Missed | Detected | ✅ |
| System Network Configuration Discovery | T1016 | Missed | Detected | ✅ |
| Scheduled Task | T1053.005 | Missed | Detected | ✅ |
| Persistent Service 1 | T1543.003 | Missed | Detected | ✅ |
| Persistent Service 2 | T1543.003 | Missed | Detected | ✅ |
| Artifact Cleanup | T1070.004 | Missed | Detected | ✅ |
| Process Injection | T1055 | Partial | Partial | ⚠️ |

## Ergebnis

- **Vor Fixes:** 0 % Detection Coverage (0 von 30)
- **Nach Fixes:** 30 % Detection Coverage (9 von 30)
- **Verbesserung:** +30 Prozentpunkte

## Erkenntnisse

1. Die ursprüngliche SwiftOnSecurity-Sysmon-Config hatte ProcessAccess **komplett deaktiviert** – ein doppelter Gap (fehlende Regel + fehlende Aktivierung).
2. Wazuh hat für viele Discovery-Befehle **keine Standard-Regeln** – Custom Rules sind zwingend erforderlich.
3. Die höchste Detektionsrate liegt bei Persistence-Techniken (T1053.005, T1543.003), weil Sysmon diese zuverlässig protokolliert.
4. Collection-Techniken (T1560.001, T1113, T1115) bleiben **komplett undetektiert** – hier besteht weiterer Handlungsbedarf.
