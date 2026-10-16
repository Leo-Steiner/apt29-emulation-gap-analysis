# Methodology

**Project:** APT29 Emulation & Detection Gap Analysis
**Author:** Leo Steiner
**Date:** October 2026

---

## 1. Approach

The project follows a five-step cycle:

1. **Research** – Select an APT group and map its TTPs to MITRE ATT&CK.
2. **Build** – Set up an isolated lab with an attacker and a defender VM.
3. **Emulate** – Execute the APT29 emulation plan with MITRE Caldera.
4. **Measure** – Compare attacker-side execution with defender-side alerts.
5. **Improve** – Implement fixes for identified gaps and re-test.

This cycle reflects the detection engineering workflow used in professional SOC environments: **build → measure → improve → repeat**.

---

## 2. APT29 Selection

APT29 (also known as Cozy Bear, Midnight Blizzard, NOBELIUM) was chosen because:

- It is one of the most thoroughly documented APT groups in MITRE ATT&CK (G0016).
- MITRE ran a dedicated ATT&CK Evaluation against it, which produced a public, structured emulation plan.
- It repeatedly targets government and diplomatic entities in Europe, giving the project a concrete EU context.
- The CTID Adversary Emulation Library provides a ready-to-use APT29 plan for Caldera.

---

## 3. Lab Design

### Architecture

- **Cloud:** Microsoft Azure (region: swedencentral)
- **VM 1 (`vm-defender`):** Ubuntu 22.04, hosts Caldera C2 server and Wazuh Manager
- **VM 2 (`vm-target`):** Windows Server 2022, hosts Sandcat agent, Wazuh Agent and Sysmon
- **Network:** Both VMs in the same VNet (`10.0.0.0/24`), private communication
- **Access:** All inbound ports restricted to operator IP via NSG rules

### Why two VMs?

- One VM acts as the attacker (`vm-defender` runs Caldera).
- One VM acts as the victim (`vm-target` runs Windows).
- The defender (Wazuh) runs on the attacker VM because it needs to collect logs from the victim over the private network.

### Why Azure?

- The task allows cloud environments.
- No local hardware is required.
- VMs can be deallocated when not in use, keeping costs low.
- Azure for Students provides free credits.

---

## 4. Emulation Setup

### Caldera

- Apache Caldera was installed from the official repository.
- The **Emu plugin** was activated to load the CTID Adversary Emulation Library.
- The **APT29 adversary profile** was assembled manually from the CTID plan, with abilities selected to match the TTP table from Phase 1.
- The Sandcat agent was deployed on `vm-target` using the private IP of `vm-defender` (`10.0.0.4:8888`).

### Wazuh + Sysmon

- Wazuh Manager, Indexer and Dashboard were installed all-in-one on `vm-defender`.
- Sysmon v15.22 was installed on `vm-target` with the SwiftOnSecurity configuration.
- The Wazuh agent on `vm-target` was configured to collect Sysmon events and Windows Security logs.
- Filebeat was configured to forward both alerts and archives to the Wazuh indexer.

---

## 5. Detection Gap Analysis

### Method

For each technique executed by Caldera:

1. Extract the timestamp and command line from the Caldera operation report (CSV).
2. Search the Wazuh Discover tab for matching events in the same time window (±30 seconds).
3. Assign a verdict:
   - **Detected:** Wazuh raised an alert with ATT&CK mapping.
   - **Partial:** Sysmon logged an event, but Wazuh raised no alert.
   - **Missed:** Neither alert nor useful telemetry.

### Result

| Metric | Value |
|---|---|
| Total techniques | 30 |
| Detected (before fixes) | 0 |
| Partial (before fixes) | 3 |
| Missed (before fixes) | 27 |
| **Coverage before fixes** | **0 %** |
| Detected (after fixes) | 13 |
| Partial (after fixes) | 3 |
| Missed (after fixes) | 14 |
| **Coverage after fixes** | **43 %** |

---

## 6. Fixes Implemented

Five detection fixes were implemented:

| Fix | Target | ATT&CK ID | Implementation |
|---|---|---|---|
| Fix 1 | Discovery commands | T1033, T1082, T1016 | Custom Wazuh rule 100200 |
| Fix 2 | LSASS ProcessAccess | T1003.001 | Sysmon ProcessAccess + ProcessAccessConfig |
| Fix 3 | Scheduled Task | T1053.005 | Custom Wazuh rules 100202–100204 |
| Fix 4 | Persistence Services | T1543.003 | Custom Wazuh rules 100205–100208 |
| Fix 5 | Collection | T1560.001, T1115, T1056.001 | Custom Wazuh rules 100210–100213 |

---

## 7. Ethical Considerations

- The lab is entirely self-owned.
- No real personal or company data was used.
- All inbound access is restricted to the operator IP.
- The lab complies with Microsoft's Cloud Penetration Testing Rules of Engagement.
- The Sandcat agent and Caldera server are not exposed to the public internet.

---

## 8. Limitations

- The emulation runs against a **single Windows host**, not a domain-joined enterprise environment.
- Some APT29 techniques (e.g., lateral movement via SMB) could not be fully emulated.
- The `Restart-Computer` command in the CTID APT29 plan interrupted the operation, which is documented as a process gap.
- Windows Defender had to be temporarily disabled for the Sandcat agent to run, which is documented as a detection gap for T1562.001.
- The detection coverage is measured against **one specific Wazuh rule set** and may differ in other environments.
