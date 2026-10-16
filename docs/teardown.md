# Teardown und Kostenkontrolle

**Datum:** 2026-10-17
**Projekt:** APT29 Emulation & Detection Gap Analysis

---

## 1. Vor dem Teardown

Alle Evidence wurde gesichert und ins GitHub-Repository gepusht:

- Caldera-Operation-CSVs (Run 1 und Run 2)
- Wazuh-Events (CSV)
- Screenshots (Azure Portal, Caldera Dashboard, Wazuh Discover)
- Detection Matrix
- Before/After Comparison
- Alle Dokumente (Methodology, Defense Recommendations, Phase-6-Summary)

## 2. Teardown-Schritte

| Schritt | Befehl | Status |
|---|---|---|
| VMs deallocaten | `az vm deallocate --ids $(...)` | ✅ |
| Ressourcengruppe löschen | `az group delete --name rg-apt29-lab --yes --no-wait` | ✅ |
| Prüfen, dass nichts läuft | `az resource list` | ✅ |
| Kostenstatus prüfen | Azure Portal → Cost Management | ✅ |

## 3. Verbleibende Ressourcen

Nach dem Teardown bleibt nur:

- `NetworkWatcherRG` mit `NetworkWatcher_swedencentral` – automatisch von Azure angelegt, kostenlos

Alle projekt-spezifischen Ressourcen wurden gelöscht:

- `rg-apt29-lab`
- VMs `vm-defender` und `vm-target`
- VNet `vnet-apt29`
- NSGs `vm-defenderNSG` und `vm-targetNSG`
- Public IPs
- OS-Disks

## 4. Kostenbilanz

### 4.1 Tatsächliche Kosten

| Metrik | Wert |
|---|---|
| Gesamtverbrauch | **3,13 USD** |
| Budget 1 (`apt29-lab-budget`) | 20,00 USD (80 % Alert bei 16,00 USD) |
| Budget 2 (`end-budget-now`) | 20,00 USD (100 % Alert bei 20,00 USD) |
| Verbrauch in % | **15,65 %** |
| Free-Trial-Guthaben | 200,00 USD |
| **Verbleibendes Guthaben** | **196,87 USD** |

### 4.2 Screenshots

**Budgets-Übersicht:**
![Budgets Overview](evidence/azure_budget_overview.png)

**Budget `apt29-lab-budget` (80 % Alert):**
![apt29-lab-budget](evidence/azure_budget_apt29.png)

**Budget `end-budget-now` (100 % Alert):**
![end-budget-now](evidence/azure_budget_end.png)

### 4.3 Aufschlüsselung der Kosten

| Ressource | Geschätzte Kosten | Anteil |
|---|---|---|
| VM `vm-defender` (Compute) | ~1,20 USD | 38 % |
| VM `vm-target` (Compute) | ~1,20 USD | 38 % |
| OS-Disks (2 × Standard SSD) | ~0,50 USD | 16 % |
| Public IPs (2 × Standard) | ~0,10 USD | 3 % |
| Netzwerk (VNet, NSGs) | 0,00 USD | 0 % |
| **Gesamt** | **~3,13 USD** | **100 %** |

**Hinweis:** Die Aufschlüsselung ist eine Schätzung. Die tatsächlichen Werte findest du in der Azure Cost Analysis.

## 5. Erkenntnisse zur Kostenkontrolle

1. **Deallocate ist essenziell.** Ohne Deallocate hätten die VMs ~5 €/Tag gekostet. Mit Deallocate lag der Gesamtverbrauch bei nur 3,13 USD für das gesamte Projekt.

2. **Budget-Alerts sind Pflicht.** Zwei Alerts (80 % und 100 %) haben frühzeitig gewarnt und das Kostenbewusstsein geschärft.

3. **Eine Ressourcengruppe für alles** macht den Teardown zu einem einzigen Befehl und verhindert vergessene Ressourcen.

4. **Private Repository** vermeidet Plagiatsrisiken und hält die Arbeit geschützt.

5. **Azure Free Trial mit 200 USD** reicht für ein Projekt dieser Größe vollständig aus. Der Verbrauch lag bei nur 1,57 % des Guthabens.

6. **Dynamische Heim-IP** erforderte mehrfache NSG-Anpassungen. Für zukünftige Projekte wäre ein VPN oder eine statische IP hilfreich.

## 6. Was erhalten bleibt

- GitHub-Repository (privat, bis Note veröffentlicht)
- Alle Evidence und Dokumente
- Azure-Konto (kann für zukünftige Projekte genutzt werden)
- Kali Linux mit installierten Tools (Azure CLI, Git, VS Code)
- Restguthaben: 196,87 USD

## 7. Empfehlungen für zukünftige Projekte

| Empfehlung | Grund |
|---|---|
| Budget-Alert vor der ersten Ressource | Verhindert unerwartete Kosten |
| `deallocate` nach jeder Session | Spart ~5 €/Tag |
| Eine Ressourcengruppe pro Projekt | Einfacher Teardown |
| Statische IP oder VPN für NSG-Regeln | Weniger manuelle Anpassungen |
| Regelmäßiger Kosten-Check im Portal | Frühzeitiges Erkennen von Ausreißern |
