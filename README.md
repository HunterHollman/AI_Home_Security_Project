#  AI-Powered Home Cybersecurity Lab 

As a Solutions Consultant with a foundation in cybersecurity, I wanted to explore a question:

> **Can we build a functional SOC (Security Operations Center) at home using only open-source tools and AI for decision-making and configuration?**

This repo contains my full implementation, from architecture to automation, powered by tools like **pfSense**, **Suricata**, **Elastic Stack**, and **GPT-4**.

---

##  What I Built

- **Virtualized pfSense Firewall** for traffic control and segmentation  
-  **VLANs** to separate trusted vs untrusted (IoT) devices  
-  **AI-Generated Rulesets** using GPT-4 for Suricata IDS  
-  **Python Script** to summarize and prioritize IDS alerts  
-  **Real-time Alert Bot** sending notifications via email/Slack  
-  **Elastic Stack Dashboard** for centralized log visibility  
-  **Simulated Attacks & AI-Assisted Response Playbooks**  

This isn't just for show — this system protects my home network and acts as a testbed for SMB-scalable cybersecurity architectures.

---


## Architecture Overview

```
                +-----------------+
                |     Modem       |
                +--------+--------+
                         |
                  [WAN Interface]
                         |
                 +-------+-------+
                 |     pfSense   |
                 +-------+-------+
                         |
          +--------------+--------------+
          |                             |
   [Trusted VLAN]                 [IoT VLAN]
   (Desktops, Phones)           (Cameras, TV, Smart Devices)

         |
   [Syslog → ELK Stack]
         |
   [AI Alert Summarizer → Notification Bot]
```

---

## Phase 1: Home Lab Setup

### Hardware/Environment

- VirtualBox
- pfSense VM with 2 NICs (WAN + LAN)


###  Network Design

| Subnet        | Purpose           |
|---------------|-------------------|
| `192.168.1.0` | Home / Trusted    |
| `192.168.2.0` | IoT / Untrusted   |
| `192.168.99.0`| Management VLAN   |

---

## Phase 2: Firewall + IDS Setup

###  pfSense Configuration

- Create VLAN interfaces
- Set DHCP and firewall rules for each VLAN
- Block east-west traffic between VLANs
- Enable DNS Resolver in pfSense



## Phase 3: Logging + Detection

### Log Pipeline

1. pfSense → syslog → Filebeat → Logstash
2. Logstash parses and tags → Elasticsearch
3. Kibana dashboards for:
   - VLAN traffic
   - Blocked packets by rule
   - Suricata alerts by category
   - Top talkers, protocols, and ports

---

## Phase 4: Alert Processing with AI

###  Python Script – Alert Summary

`alert_summarizer.py`

- Parses Suricata JSON logs
- Groups by IP, alert type, severity
- Flags potential threats (based on thresholds)
- Summarizes results for Slack/email

```bash
python alert_summarizer.py /var/log/suricata/eve.json
```

###  Notification Bot

- Sends digest to Slack or email daily
- Future: GPT auto-response suggestions based on log pattern

---

##  Phase 5: Simulated Incidents & AI IR Playbooks

### Attack Scenarios

- SSH brute-force
- DNS tunneling (iodine)
- Lateral movement attempts (SMB scans)

### AI-Powered IR Workflow 

 Output:
- Isolate host
- Reset credentials
- Block offending IP range
- Review lateral logs from source IP
- Document and file report

---

## 🗂️ Repo Structure

```
/config/
    pfSense_rules.xml
    suricata_custom.rules
    elastic_pipeline.conf
/scripts/
    alert_summarizer.py
    send_alerts.py
/diagrams/
    home_soc_architecture.png
docs/
    prompts.md
    ir_playbooks.md
README.md
```

---

##  Scaling to SMBs

This architecture can easily scale:
- Add multi-user auth via VPN
- Deploy ELK on AWS, Azure
- Centralize Suricata feeds from remote offices
- Use GPT for ticket triage or SOAR-like workflows

---






---

## 🔗 Connect with Me

👤 **Hunter Hollman**  
💼 Associate Solutions Consultant @ Aptean  
🔐 Cybersecurity | Cloud | ERP | AI | SOC  
📧 [LinkedIn](https://www.linkedin.com/in/hunter-hollman-29b49a227/) 

