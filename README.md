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

## 🔧 Network Configuration Table

| Component      | Adapter Type     | Network Name     | IP Address        | Purpose                                |
|----------------|------------------|------------------|-------------------|----------------------------------------|
| **Host PC**    | —                | —                | Dynamic (192.168.x.x) | Hosts VirtualBox; isolated from `LAB-NET` |
| **Kali VM**    | Adapter 1: NAT   | NAT              | DHCP (dynamic)    | Internet access for tools/updates     |
|                | Adapter 2: Internal | LAB-NET       | `192.168.56.10`   | Isolated attacker interface           |
| **Windows VM** | Adapter 1: NAT   | NAT              | DHCP (dynamic)    | Internet for patching, RDP, etc.      |
|                | Adapter 2: Internal | LAB-NET       | `192.168.56.20`   | Target machine, pentesting surface    |
| **LAB-NET**    | Internal Only    | LAB-NET          | `192.168.56.0/24` | No internet, no host access           |

---
                    +----------------------+
                    |      Host PC         |
                    | (VirtualBox Host)    |
                    +----------------------+
                   [Internet Access via NAT]
                      (hosted by VirtualBox)
                              |
                 +------------+------------+
                 |                         |
          +-------------+           +-------------+
          |   Kali VM   |           | Windows VM  |
          |-------------|           |-------------|
          | Adapter 1:  |           | Adapter 1:  |
          | NAT         |           | NAT         |
          |-------------|           |-------------|
          | Adapter 2:  |           | Adapter 2:  |
          | LAB-NET     |<--------->| LAB-NET     |
          | 192.168.56.10|          | 192.168.56.20|
          +-------------+           +-------------+

              🔒 Internal Network (LAB-NET)
           - Subnet: 192.168.56.0/24
           - Isolated from host and internet
           - Used for pen testing, scanning, attacks




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


# 🌐 Home Cybersecurity Lab Network Topology

This section documents the exact networking setup for my home SOC and pentest lab using VirtualBox.

Each VM has **two adapters**:
- One for **internet access** via **NAT**
- One for **internal, isolated penetration testing** via **Internal Network: LAB-NET**

---


---

## 🔗 Connect with Me

👤 **Hunter Hollman**  
💼 Associate Solutions Consultant @ Aptean  
🔐 Cybersecurity | Cloud | ERP | AI | SOC  
📧 [LinkedIn](https://www.linkedin.com/in/hunter-hollman-29b49a227/) 

