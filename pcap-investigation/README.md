# Network Forensics: PCAP Threat Investigation

Step-by-step analysis of a packet capture to find a suspected "Bandit Yeti" intrusion.
This was a practice exercise using a training capture (`AoC'22-Day13.pcapng`), not a real incident.

**Report:** [pcap-investigation-report.pdf](pcap-investigation-report.pdf) (10 pages, with screenshots)

## Method
1. **Overview:** Wireshark Protocol Hierarchy and Conversations. Over 1,000 packets used port 3389 (RDP), and a small amount of HTTP stood out
2. **DNS:** filtered DNS and found a suspicious domain, `cdn[.]bandityeti[.]thm`
3. **HTTP:** found an internal host downloading `mysterygift.exe` and `favicon.ico`, the latter with an unusual `Nim httpclient` user-agent
4. **File extraction:** exported the executable with File > Export Objects > HTTP and calculated its SHA-256
5. **Threat intelligence:** looked up the hash on VirusTotal and found its command-and-control IPs

## Indicators of compromise (defanged)
| Type | Value |
|---|---|
| Internal host | 10[.]10[.]29[.]186 |
| Domain | cdn[.]bandityeti[.]thm |
| File | mysterygift[.]exe |
| SHA-256 | 0ce160a54d10f8e81448d0360af5c2948ff6a4dbb493fe4be756fc3e2c3f900f |
| C2 IPs | 20[.]99[.]133[.]109, 23[.]216[.]147[.]69 |

## Recommendations
Isolate the host, block the domain and C2 IPs at the firewall, and hunt for the hash and C2 connections across the network.
