# Splunk Log Analysis: SSH and Web Server Security

Investigating brute-force activity and web errors in Splunk's sample log data, then checking whether any attacker got in.

**Report:** [splunk-log-analysis-report.pdf](splunk-log-analysis-report.pdf) (20 pages, with queries and dashboard)

## Dataset
Splunk sample data (`tutorialdata.zip`): 109,864 events across 8 files. Four are SSH `secure.log` files and three are Apache `access.log` files. The vendor sales file is business data and was left out.
This is simulated data, so the findings show technique and are not a real incident.

## What I did
- Counted events per file and extracted fields (IP, user, status code) manually with `rex`, because Splunk did not extract them
- Ranked failed SSH logins by source IP and checked for any successful login from those IPs
- Compared failures and successes per IP to look for a successful brute force
- Analysed Apache status codes, error rates per page and requests for non-existent files
- Linked SSH attackers to web traffic and charted failed logins per day
- Built a 4-panel dashboard, "SSH and Web Security Overview"

## Key findings
- 33,253 failed SSH logins. 33,069 came from 182 external IPs, at a steady rate of about 4,700 per day
- 1,599 successful logins, all from 3 internal users. **No external IP ever logged in**
- Web traffic: 39,532 requests, 5,250 errors (13.3%), spread evenly across pages
- 8 non-existent paths (including `/passwords.pdf`) were requested 610 times and always returned 404, so nothing was exposed
- Session IDs appear in URLs, which is poor practice

## Data-quality lessons
- **Duplicate ingestion:** the zip had been loaded three times, which inflated counts. I found it by checking `_indextime` and filtered to one clean copy
- **Case-sensitive regex:** 184 lowercase "failed password" events matched the keyword search but not `rex`. I found the gap by reconciling the totals and fixed it with `(?i)`

## Recommendations (if this were real)
Block or rate-limit attacking IPs, use key-based SSH and disable root login, deploy fail2ban, alert on many failed logins from one IP, and remove session IDs from URLs.

## Limitations
Key-based logins (`Accepted publickey`), intra-day timing and attacker geography were not analysed.
