# Phase 6 – Gap-Analyse, Fixes und Run 2 Vergleich

## Ausgangslage

Run 2 wurde am 2026-10-16 von 18:19:44 bis 18:59:55 UTC ausgeführt. Der Test umfasste 35 Fähigkeiten aus dem CTID-APT29-Emulationsplan. Der Lauf wurde durch den `Restart-Computer`-Befehl in der Ability „Scheduled Task" unterbrochen.

## Gap-Analyse

Die Attacker-Seite (Caldera-CSV) wurde mit der Defender-Seite (Wazuh-Alerts) verglichen. Für jede Fähigkeit wurde ein Verdikt vergeben:
- **Detected:** Wazuh hat einen Alert mit ATT&CK-Mapping ausgelöst
- **Partial:** Sysmon hat ein Event protokolliert, aber Wazuh hat keinen Alert ausgelöst
- **Missed:** Weder Alert noch nützliche Telemetrie

## Implementierte Fixes

| Fix | Ziel | ATT&CK ID | Umsetzung |
|---|---|---|---|
| Fix 1 | Discovery-Befehle | T1033, T1082, T1016 | Custom Wazuh-Regel 100200 |
| Fix 2 | LSASS ProcessAccess | T1003.001 | Sysmon ProcessAccess + ProcessAccessConfig |
| Fix 3 | Scheduled Task | T1053.005 | Custom Wazuh-Regeln 100202–100204 |
| Fix 4 | Persistence Services | T1543.003 | Custom Wazuh-Regeln 100205–100208 |

## Ergebnis

- **Vor Fixes:** 0 % Detection Coverage (0 von 30)
- **Nach Fixes:** 30 % Detection Coverage (9 von 30)
- **Verbesserung:** +30 Prozentpunkte

## Dokumentierte Gaps

Die folgenden Techniken bleiben auch nach den Fixes undetektiert:
- Collection: T1560.001, T1113, T1115
- Discovery: T1057, T1007, T1069, T1087, T1012, T1018, T1049
- Defense Evasion: T1036, T1112
- Credential Access: T1555.003, T1552.004

## Empfehlungen

1. Weitere Custom Wazuh-Regeln für Collection-Techniken (T1560.001, T1113, T1115).
2. Sysmon-Config um Clipboard-Monitoring (Event ID 24) erweitern.
3. Windows-Audit-Policy für detailliertere Prozess-Zugriffe aktivieren.
4. Regelmäßige Überprüfung der Detection Coverage mit aktualisierten Emulationsplänen.
