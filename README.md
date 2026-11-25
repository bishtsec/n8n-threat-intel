# IP Threat Intelligence Check — n8n Workflow

A cybersecurity SOAR (Security Orchestration, Automation and Response) workflow built on [n8n](https://n8n.io) that checks any IP address against multiple threat intelligence sources and delivers an automated verdict.

## What It Does

1. **User submits an IP address** via a web form
2. **VirusTotal API** checks the IP against 90+ antivirus engines for malicious/suspicious flags
3. **AbuseIPDB API** checks the IP for abuse reports, confidence score, and ISP information
4. **Verdict Engine** (Code node) classifies the IP as:
   - **MALICIOUS** — if any VirusTotal engine flags it as malicious, or AbuseIPDB score >= 50%
   - **SUSPICIOUS** — if any engine flags it as suspicious, or AbuseIPDB score >= 25%
   - **CLEAN** — otherwise
5. **Email Report** sends a color-coded HTML summary via Gmail

## Workflow Architecture

```
[Web Form] → [VirusTotal API] → [AbuseIPDB API] → [Build Verdict] → [Gmail Report]
```

## Live Demo

**Try it here:** [IP Threat Intelligence Check](https://xku.app.n8n.cloud/form/ip-threat-intel-check)

Enter any IP address and the workflow runs in real-time (~2 seconds).

### Test IPs

| IP | Expected Verdict | Why |
|----|-----------------|-----|
| `185.220.101.1` | MALICIOUS | Known Tor exit node, 100% abuse score |
| `8.8.8.8` | CLEAN | Google Public DNS |
| `1.1.1.1` | CLEAN | Cloudflare DNS |

## Setup (Import Into Your Own n8n)

1. Open your n8n instance
2. Go to **Workflows** → click the **three-dot menu** → **Import from File**
3. Select `IP_Threat_Intelligence_Check.json`
4. Add your credentials:
   - **VirusTotal API Key** — free at [virustotal.com](https://www.virustotal.com/gui/join-us)
   - **AbuseIPDB API Key** — free at [abuseipdb.com](https://www.abuseipdb.com/register)
   - **Gmail** — connect your Google account via OAuth2
5. Update the email address in the "Email Summary" node
6. Click **Publish** and **Execute**

## Sample Output

For IP `185.220.101.1`:

- **VirusTotal:** 12 engines flagged malicious, 5 suspicious
- **AbuseIPDB:** Abuse score 100%, 486 reports from 133 users, known Tor exit node
- **Verdict: MALICIOUS**
- **Email:** Color-coded HTML report sent to inbox

## Tech Stack

- **n8n** — workflow automation platform
- **VirusTotal API v3** — malware/threat intelligence
- **AbuseIPDB API v2** — IP abuse/reputation database
- **Gmail API** — email delivery

## Author

Aditya Bisht
