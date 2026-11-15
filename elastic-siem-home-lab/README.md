# Elastic Security SIEM Home Lab

A small detection lab that sends network data (Zeek) and endpoint data (Elastic Defend, Windows integration, Sysmon) to Elastic Cloud, then searches, alerts and investigates in Elastic Security.

**Report:** [elastic-siem-home-lab-report.pdf](elastic-siem-home-lab-report.pdf) (11 pages, with screenshots)

## Lab environment
| Component | Details |
|---|---|
| SIEM | Elastic Cloud Security project (trial), Fleet |
| Ubuntu VM | Zeek with ZeekControl, Elastic Agent |
| Windows 11 VM | Elastic Agent 9.5.5 with System, Elastic Defend and Windows integrations, Sysmon v15.22 |
| Parrot OS VM | Nmap 7.98 scanner on the host-only network |

Both agents showed Healthy in Fleet.

## What I did
- Deployed Zeek and confirmed connection and DNS logs arriving in Discover (about 4,000 connection events per 15 to 30 minutes)
- Ran an Nmap service scan from Parrot OS against the Windows VM. All 1000 ports came back filtered
- Downloaded a harmless malware test file. Elastic Defend raised a **critical alert (risk score 99)** and terminated it
- Used the Analyzer to read the process tree: `winlogon.exe` → `userinit.exe` → `explorer.exe` → `msedge.exe` → the test file
- Ran PowerShell commands (`whoami`, `ipconfig`, `Get-ComputerInfo`) and found them as process, file and PowerShell log events
- Installed Sysmon with a community config and confirmed 457 Sysmon events in 12 hours, including process creation and network connections

## Troubleshooting log
The report documents five problems and fixes, such as the Analyzer showing no events before ingestion caught up, wrong field names in queries, and Discover pointing at the wrong data view.

## Next steps
Pivot on Sysmon network events, find web and DNS traffic in the Zeek datasets, and write a custom detection rule (for example `whoami.exe` launched from PowerShell).
