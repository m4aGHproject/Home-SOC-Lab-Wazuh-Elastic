# Home SOC Lab — Wazuh + Elastic

A fully deployed at-home Security Operations Center for detection engineering, alert triage, and threat-hunting practice.  
Runs Wazuh + Elastic Stack with Windows and Linux endpoints feeding real telemetry.

---

## ⚙️ Stack Overview
- **Wazuh Manager** — endpoint monitoring, agents, rules, decoders
- **Elastic Stack (Elasticsearch + Kibana)** — log storage, dashboards, hunting visualizations
- **Windows & Linux agents** — Sysmon, auditd, PowerShell logs, file integrity
- **Docker-based deployment** — fast, repeatable lab setup

---

## 🎯 Lab Goals
- Build & tune custom detection rules  
- Map detection logic to MITRE ATT&CK  
- Hunt malicious behaviors  
- Collect Windows & Linux telemetry  
- Produce a portfolio-ready SOC environment  

---

## 🚀 Deployment
```bash
git clone https://github.com/YOURUSER/home-soc-lab-wazuh-elastic
cd home-soc-lab-wazuh-elastic
docker-compose up -d
