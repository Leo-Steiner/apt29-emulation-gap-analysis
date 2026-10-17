# APT29 Emulation & Detection Gap Analysis

**A self-owned Azure lab project emulating APT29 (Cozy Bear / Midnight Blizzard) with MITRE Caldera, measuring detection gaps with Wazuh + Sysmon, closing them with custom rules, and re-testing.**

---

## Headline Result

**Detection Coverage improved from 0 % to 30 % after implementing 5 detection fixes.**

- **Initial state (Run 2):** 0 of 30 techniques detected
- **After fixes:** 9 of 30 techniques detected
- **Improvement:** +30 percentage points

---

## What this project does

1. **Emulates APT29** in a controlled Azure lab using the CTID Adversary Emulation Library and MITRE Caldera.
2. **Measures detection coverage** with Wazuh SIEM/XDR and Sysmon telemetry.
3. **Identifies detection gaps** by comparing the attacker-side execution log (Caldera) with the defender-side alerts (Wazuh).
4. **Implements fixes** — custom Wazuh rules and Sysmon config changes — to close the most critical gaps.
5. **Re-tests** and quantifies the improvement.

---

## Architecture

<img width="633" height="481" alt="image" src="https://github.com/user-attachments/assets/25f242b9-4825-469e-8625-d9e048fded41" />


**Diagram:** See `docs/architecture.png`.

---

## Tools used

| Tool | Purpose | Version |
|---|---|---|
| Microsoft Azure | Cloud infrastructure | – |
| MITRE Caldera | Adversary emulation framework | 5.x (Apache) |
| CTID Emu Plugin | Loads APT29 emulation plan | – |
| Wazuh | SIEM / XDR for detection | 4.14.9 |
| Sysmon | Windows telemetry | 15.22 |
| MITRE ATT&CK Navigator | Heatmap visualization | – |
| Kali Linux | Operator workstation | 2026.x |

---

## Detection Coverage Results

| Metric | Run 2 (before fixes) | After fixes |
|---|---|---|
| Total techniques | 30 | 30 |
| Detected | 0 | 9 |
| Partial | 3 | 3 |
| Missed | 18 | 14 |
| **Coverage** | **0 %** | **30 %** |

**Heatmaps:** See `navigator/run2_heatmap_before.json` and `navigator/run2_heatmap_after.json`.

---

## Implemented Fixes

| Fix | Target | ATT&CK ID | Implementation |
|---|---|---|---|
| Fix 1 | Discovery commands | T1033, T1082, T1016 | Custom Wazuh rule 100200 |
| Fix 2 | LSASS ProcessAccess | T1003.001 | Sysmon ProcessAccess + ProcessAccessConfig |
| Fix 3 | Scheduled Task | T1053.005 | Custom Wazuh rules 100202–100204 |
| Fix 4 | Persistence Services | T1543.003 | Custom Wazuh rules 100205–100208 |
| Fix 5 | Collection | T1560.001, T1115, T1056.001 | Custom Wazuh rules 100210–100213 |

**Rule file:** See `detection/custom-wazuh-rules.xml` (12 custom rules across 5 fixes).

---

## Repository Structure
apt29-emulation-gap-analysis/
├── README.md ← you are here
├── LICENSE ← MIT
├── docs/
│ ├── apt29-profile.md ← group profile + justification
│ ├── attack-scenario.md ← attack narrative
│ ├── methodology.md ← approach in own words
│ ├── architecture.png ← lab diagram
│ ├── phase6-summary.md ← gap analysis summary
│ └── defense-recommendations.md ← technical + non-technical
├── detection/
│ ├── custom-wazuh-rules.xml ← 13 custom rules
│ └── sysmon-config-notes.md ← Sysmon config changes
├── adversary/
│ └── apt29-profile-notes.md ← TTP table with ATT&CK IDs
├── navigator/
│ ├── run2_heatmap_before.json ← before fixes
│ └── run2_heatmap_after.json ← after fixes
└── evidence/
├── phase2_azure_portal_baseline.png
├── phase3_agent_active.png
├── caldera_dashboard.png
├── caldera_agents.png
├── wazuh_discover_alerts1.png
├── wazuh_discover_alerts2.png
├── run2_caldera_operation.csv ← attacker-side ground truth
├── run2_detection_matrix.md ← 30 techniques with verdicts
└── run2_before_after.md ← before/after comparison


---

## What I learned

- **Telemetry without rules is not detection.** Sysmon logs everything, but without Wazuh rules that match specific patterns, no alert is raised. The gap between "raw telemetry" and "actionable alert" is the core of detection engineering.
- **Default configurations are not enough.** The widely used SwiftOnSecurity Sysmon config had `ProcessAccessConfig` completely disabled. LSASS dumps were invisible until I explicitly enabled it.
- **Quantifying detection coverage is a differentiator.** Most emulation exercises stop at "we ran the attack." Measuring before/after coverage makes the result comparable and actionable.
- **Windows Server 2022 has quirks.** Some Sysmon features (like ProcessAccess) require explicit activation and don't behave like on Windows 10/11.
- **Reboot-inducing abilities break emulations.** The `Restart-Computer` command in the CTID APT29 plan killed the Sandcat agent mid-operation. This is a process gap, not a detection gap.

---

## Ethical scope

- This lab was **entirely self-owned** in a private Azure subscription.
- All inbound connections were restricted to the operator's IP address.
- No real personal or company data was used.
- The lab was dismantled after the project to avoid ongoing costs.
- The project complies with the **Microsoft Cloud Penetration Testing Rules of Engagement**.

---

## Sources

- MITRE ATT&CK – APT29 (G0016): https://attack.mitre.org/groups/G0016/
- CTID Adversary Emulation Library: https://github.com/center-for-threat-informed-defense/adversary_emulation_library
- MITRE Caldera: https://github.com/apache/caldera
- Wazuh Documentation: https://documentation.wazuh.com/
- Sysmon: https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon

---

## License

MIT – see `LICENSE`.

---

## Status

**Private until grade is published.** This repository does not contain the IU examination task text or the full submitted report. It shows the lab and the craft, in my own words.

