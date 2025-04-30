# UNRAID SCRIPTS

This is my repo for my private Unraid media server setup. It includes custom tools and scripts I've developed to enhance performance, monitoring, and automation — beyond what Unraid provides out of the box.

### 📜 Custom User Scripts Included

1. **🔍 HDD SMART Health Monitoring**  
   Deep-dive SMART analyzer with firmware warning checks and Unraid notification integration.

2. **⚡ PCIe ASPM Diagnostics**  
   Detects ASPM capability, status, and known blockers (e.g., LSI, ASM, PLX) with actionable summaries and notification support.

---

### 🔍 HDD SMART Health Monitoring Script

This script performs a comprehensive audit of SMART data from all connected `/dev/sd?` drives in Unraid, offering **deeper and more contextual analysis than Unraid's built-in SMART monitoring**. While Unraid notifies users about raw SMART failures, it does **not** evaluate drive longevity trends, firmware-level risks, or usage pattern mismatches—critical for aging or repurposed drives in media servers.

For example, the drives in my Unraid array had abnormally high head park (load/unload) counts due to running 24/7 in a Windows desktop before being migrated to this server. These kinds of wear indicators are not flagged by Unraid, yet they can point to reliability issues long before a SMART failure is triggered.

**Key Features:**

- **Classifies** drives based on usage class (Enterprise, NAS, Surveillance)
- **Evaluates SMART attributes**, including:
  - Power-On Hours (POH)
  - Load/Unload cycle count (head parking)
  - Reallocated and pending sector counts
  - Operating temperature
- **Flags potential risks**, such as:
  - Excessive wear / nearing lifespan limits
  - High thermal exposure
  - Emerging SMART errors
- **Outputs warnings for known firmware bugs**, including severity and links to vendor/community advisories
- **Integrates with Unraid notifications** for health issues (excludes firmware warnings to avoid false alerts)
- **Identifies unclassified drive models** to refine logic for future updates

> 📂 Script path: `user.scripts/HDD_SMART_Status_Snapshot` (name it however you like)  
> 🕒 Recommended: Run daily or weekly using Unraid’s User Scripts plugin

---

**Example Script Description (for Unraid UI):**

```text
Scans all drives for SMART metrics, flags health issues like bad sectors, high temperatures, and near-EOL wear. Classifies drive types (e.g. Exos, Red, SkyHawk) and estimates remaining life. Also checks firmware against a database of known bugs (e.g. SN04 EPC issue). Sends Unraid alerts for health issues only. Firmware warnings shown in console output only.
```
### ⚡ PCIe ASPM Diagnostics Script

This script audits **PCIe Active State Power Management (ASPM)** support and status across all PCIe devices in your Unraid system. Unlike typical ASPM tools, it analyzes not just link state, but also:

- **Capability detection** (`ASPM`, `ASPMOptComp+`)
- **Enablement status**
- **Known hardware blockers** (e.g. LSI, PLX, ASMedia)
- **Fixable misconfigurations** (e.g. Intel root ports that support ASPM but aren't using it)

It also integrates with:
- ✅ **Unraid notifications** (severity-based)
- ✅ **Syslog logging**
- ✅ **Optional CSV export** to `/boot/logs/aspm_report.csv`
- ✅ **Summary counters** and detailed recommendations
- ✅ **Formatted output** compatible with User Scripts UI or terminal

> 📂 Script path: `user.scripts/PCIe_ASPM_Diagnostics`  
> 🕒 Recommended: Run at boot, after BIOS updates, or after kernel or HBA updates  
> ⚙️ Optional Pre-Req: [User Scripts plugin](https://forums.unraid.net/topic/48286-plugin-ca-user-scripts/) for scheduling and UI execution
---
**Example Script Description (for Unraid UI):**

```text
Scans all PCIe devices and reports ASPM status (Active State Power Management). Highlights devices that block ASPM or support it but are disabled. Useful for reducing idle power and debugging high wattage issues in always-on servers. Suggests boot parameters and hardware-level fixes where applicable.
```
**Example Output (User Scripts terminal):**
```
🔹 00:1c.1 - Intel PCIe Root Port #2
    OptComp      : Yes
    Capable      : Yes
    Enabled      : No
    Recommendation: 🔧 Try pcie_aspm=force
--------------------------------------------------------------------------

🔹 0c:00.0 - LSI SAS3008 PCIe HBA
    OptComp      : Yes
    Capable      : Yes
    Enabled      : No
    Recommendation: ❌ LSI blocks ASPM
--------------------------------------------------------------------------

📊 Summary:
  Devices scanned: 36
  ASPM Enabled: 14
  ASPM Disabled but fixable (Try): 1
  Blocked (LSI/ASM/PLX): 7
  Unsupported: 14
```
