# UNRAID-PLEX

This is my repo for my private UNRAID media server. It will include anything custom that I have built into my setup.

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
