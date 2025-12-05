├── agents/
│   ├── windows/# Windows Agent Install Guide

1. Download the Wazuh agent from:   `https://wazuh.com/downloads`
2. Install using the default settings.
3. Add the Wazuh manager address:
4. Enable PowerShell logging: - Module logging - Script block logging - Transcription
5. Install Sysmon: ``` sysmon64.exe -accepteula -i sysmon.xml```
6. Confirm logs are flowing. ```

│   ├── linux/
# Linux Agent Install Guide

```bash curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH |

sudo apt-key add -
sudo apt-get install wazuh-agent
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

Enable auditd:

sudo apt install auditd
sudo systemctl enable auditd
sudo systemctl start auditd

Confirm agent: /var/ossec/bin/agent-control -i

│   └── hardening-checklist.md

# `/agents/hardening-checklist.md`

```markdown
# Endpoint Hardening Checklist

- Enable Sysmon (Windows)
- Enable PowerShell logging
- Enable Windows Defender MAPS
- Disable unnecessary services
- Apply auditd rules (Linux)
- Enforce SSH key auth (optional)
