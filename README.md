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
| Missed | 27 | 18 |
| **Coverage** | **0 %** | **30 %** |


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

<img width="707" height="761" alt="output_tree" src="https://github.com/user-attachments/assets/95bfb994-4e89-46e4-8a0b-4190e8cdbb1e" />



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

