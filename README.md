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
Access
Wazuh Dashboard: http://localhost:5601

Kibana: http://localhost:5601

Elasticsearch: http://localhost:9200

📁 Repo Structure
arduino
Copy code
home-soc-lab-wazuh-elastic/
├── README.md
├── architecture/
│   ├── network-diagram.png
│   ├── wazuh-architecture.png
│   └── elastic-flow.png
├── deployment/
│   ├── docker-compose.yml
│   ├── wazuh-manager-config/
│   └── elastic-config/
├── agents/
│   ├── windows/
│   ├── linux/
│   ├── install-guides/
│   └── hardening-checklist.md
├── detection-engineering/
│   ├── wazuh-rules/
│   ├── elastic-detections/
│   └── attack-mapping.md
├── dashboards/
├── logs-and-samples/
└── LICENSE
📜 Custom Detections Included
Suspicious PowerShell execution

Mimikatz-style LSASS access

Brute force login attempts

Malware persistence via Run keys

Linux privilege escalation attempts

🧪 Sample Data
Sanitized logs from:

Windows Defender detections

Sysmon 13 events

Linux sudo abuse

SSH brute force attempts

🛡 MITRE Mappings
See /detection-engineering/attack-mapping.md for rule → technique mappings.

📸 Diagrams
See /architecture/ for visual layout of the environment.

📣 Contributions
Fork away. Add new rules. Break stuff. Detect stuff. Fix stuff.

📄 License
MIT License

yaml
Copy code

---

# `/deployment/docker-compose.yml`

```yaml
version: "3.8"
services:
  wazuh.manager:
    image: wazuh/wazuh:4.7.0
    ports:
      - "1514:1514/udp"
      - "1515:1515"
      - "55000:55000"
    volumes:
      - ./wazuh-manager-config:/var/ossec/data
    networks:
      - soc

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.11.0
    environment:
      - discovery.type=single-node
      - ELASTIC_PASSWORD=changeme
    ports:
      - "9200:9200"
    networks:
      - soc

  kibana:
    image: docker.elastic.co/kibana/kibana:8.11.0
    depends_on:
      - elasticsearch
    ports:
      - "5601:5601"
    networks:
      - soc

networks:
  soc:
    driver: bridge
/detection-engineering/wazuh-rules/custom-rules.xml
xml
Copy code
<group name="custom_rules,">
    <rule id="900001" level="8">
        <decoded_as>json</decoded_as>
        <description>Suspicious PowerShell Command Detected</description>
        <match>Powershell.exe</match>
        <regex>Invoke-WebRequest|IEX|FromBase64String</regex>
        <mitre>
            <id>T1059.001</id>
        </mitre>
    </rule>

    <rule id="900002" level="10">
        <description>LSASS Access Attempt (Possible Mimikatz)</description>
        <match>lsass.exe</match>
        <regex>process_access</regex>
        <mitre>
            <id>T1003.001</id>
        </mitre>
    </rule>

    <rule id="900003" level="7">
        <description>Brute Force Authentication Attempt</description>
        <regex>failed password|authentication failure</regex>
        <frequency>5</frequency>
        <timeframe>60</timeframe>
        <mitre>
            <id>T1110</id>
        </mitre>
    </rule>
</group>
/detection-engineering/wazuh-rules/decoders.xml
xml
Copy code
<decoder name="powershell_event">
    <program_name>Powershell</program_name>
    <type>json</type>
</decoder>

<decoder name="linux_auth">
    <program_name>sshd</program_name>
</decoder>
/detection-engineering/elastic-detections/kibana-detections.ndjson
json
Copy code
{
  "type": "security-rule",
  "name": "Suspicious PowerShell Execution",
  "query": "process.executable: *powershell.exe AND process.command_line: (*IEX* OR *Base64*)",
  "severity": "high",
  "mitre": ["T1059.001"]
}
/detection-engineering/attack-mapping.md
markdown
Copy code
# MITRE ATT&CK Mapping

| Rule Name | Description | MITRE Technique |
|----------|-------------|-----------------|
| Suspicious PowerShell Command | Encoded or remote execution | T1059.001 |
| LSASS Access Attempt | Credential dumping | T1003.001 |
| Brute Force Authentication | Password guessing | T1110 |
| Run Key Persistence | Registry modification | T1547 |
| Linux Priv Escalation | sudo/su abuse | T1068 |
/agents/windows/install-guide.md
markdown
Copy code
# Windows Agent Install Guide

1. Download the Wazuh agent from:
   `https://wazuh.com/downloads`
2. Install using the default settings.
3. Add the Wazuh manager address:
<address>YOUR_WAZUH_IP</address> ``` 4. Enable PowerShell logging: - Module logging - Script block logging - Transcription 5. Install Sysmon: ``` sysmon64.exe -accepteula -i sysmon.xml ``` 6. Confirm logs are flowing. ```
/agents/linux/install-guide.md
markdown
Copy code
# Linux Agent Install Guide

```bash
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo apt-key add -
sudo apt-get install wazuh-agent
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
Enable auditd:

bash
Copy code
sudo apt install auditd
sudo systemctl enable auditd
sudo systemctl start auditd
Confirm agent:

bash
Copy code
/var/ossec/bin/agent-control -i
yaml
Copy code

---

# `/agents/hardening-checklist.md`

```markdown
# Endpoint Hardening Checklist

- Enable Sysmon (Windows)
- Enable PowerShell logging
- Enable Windows Defender MAPS
- Disable unnecessary services
- Apply auditd rules (Linux)
- Enforce SSH key auth (optional)
dashboards/*.ndjson
(Mock export placeholders — ready for real data later.)

json
Copy code
{"dashboard":"Wazuh Overview","objects":[...]}
{"dashboard":"Threat Hunting","objects":[...]}
{"dashboard":"Windows Sysmon","objects":[...]}
logs-and-samples/
(Empty folders; add your sanitized logs.)

powershell
Copy code
logs-and-samples/
├── sanitized-windows-logs/
├── sanitized-linux-logs/
└── test-malware-events/
