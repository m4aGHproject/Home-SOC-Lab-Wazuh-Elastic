# Home SOC Lab — Wazuh + Elastic

A fully deployed at-home Security Operations Center for detection engineering, alert triage, and threat-hunting practice.  
Runs Wazuh + Elastic Stack with Windows and Linux endpoints feeding real telemetry.

---

⚙️ Stack

Wazuh Manager — log collection, rules engine, endpoint monitoring

Elastic Stack (ES/Kibana) — storage, dashboards, visualizations

Windows + Linux endpoints — real hosts, real logs

Docker-based deployment — reproducible, portable

🎯 Purpose

Practice real detection engineering

Build and tune custom Wazuh rules

Map detections to MITRE ATT&CK

Hunt through real endpoint telemetry

Show recruiters you’re not just “studying” — you’re actually doing security

🚀 How to Deploy

Clone the repo

Install Docker

Run docker-compose up -d

Access Wazuh UI → configure agents

Access Kibana → import dashboards

🔍 What’s Included

Attack simulation logs (sanitized)

Custom rules for brute force, persistence abuse, suspicious PowerShell, lateral movement

Sigma-based Elastic detections

Visual dashboards for triage workflow

📸 Diagrams

Include 2–3 clean PNG diagrams. Recruiters love visuals.
