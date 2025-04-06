# UNRAID-PLEX

This is my repo for my private UNRAID media server. It will include anything custom that I have built into my setup. This is separate from my Proxmox hypervisor-based homelab, which is in another repo here: [docker-compose](https://github.com/ALEX8642/docker-compose).

---

### 🔍 HDD SMART Health Monitoring Script

This script audits SMART data from all connected `/dev/sd?` drives in Unraid, evaluating their condition for early warning signs of potential failure. It is ideal for high-capacity media servers running 24/7 workloads such as Plex, where drive reliability is critical.

**Features:**

- Classifies drives by workload profile (Enterprise, NAS, Surveillance)
- Analyzes:
  - Power-On Hours (POH)
  - Load/Unload cycle counts (head parking activity)
  - Temperature
  - Reallocated and pending sectors
- Flags:
  - Excessive wear or nearing drive lifespan
  - Thermal issues
  - Bad sectors or SMART reallocations
- Integrates with Unraid’s native notification system (only for critical health issues)
- Independently identifies and outputs **firmware-specific warnings**, including severity and source links (only printed to console)
- Tracks unknown drive models for future improvements to classification logic

> Script path: `user.scripts/HDD_SMART_Status_Snapshot #name this whatever you want`  
> Schedule recommendation: Run daily or weekly via the User Scripts plugin in Unraid

**Suggested Unraid script description:**

```text
Scans all drives for SMART metrics, flags health issues like bad sectors, high temperatures, and near-EOL wear. Classifies drive types (e.g. Exos, Red, SkyHawk) and estimates remaining life. Also checks firmware against a database of known bugs (e.g. SN04 EPC issue). Sends Unraid alerts for health issues only. Firmware warnings shown in console output only.
