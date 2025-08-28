# Windows Event Log Analysis with ELK Stack  

## 📌 Project Overview  
This project demonstrates the use of the **Elastic Stack (Elasticsearch, Kibana, Fleet, and Elastic Agent)** to monitor and analyze **Windows Event Logs** in a sandboxed environment.  

- **Host Machine (Windows 11):** Runs Elasticsearch, Kibana, and Fleet Server  
- **Virtual Machine (Windows Server 2019 on VirtualBox):** Runs Sysmon + Elastic Agent  
- **Security Testing:** Elastic Defend (EDR) + Atomic Red Team  
- **Goal:** Provide replicable, open-source lab for Windows log analysis  

---

## 🧩 Background & Motivation  
Windows Event Logs hold valuable insights for:  
- Security monitoring  
- Troubleshooting  
- Performance analysis  
- Compliance  

**Challenges:**  
- High log volume  
- Filtering and parsing  
- Correlation and visualization  

---

## ❓ Problem Statement  
How can Windows Event Log data be analyzed and visualized in a manner that is **interactive, intuitive, and useful** for system administrators?  

---

## 🎯 Aims & Objectives  

### Aim  
To build a working monitoring system for Windows Event Logs using the Elastic Stack, enabling interactive log querying, visualization, and threat detection.  

### Objectives  
- Research tools and techniques for handling Windows Event Logs  
- Prepare and structure raw Windows log data  
- Deploy a working Elastic Stack architecture  
- Install Sysmon and Elastic Agent for log collection  
- Add integrations (Windows, Elastic Defend) for endpoint monitoring  
- Test with **Atomic Red Team** to simulate MITRE ATT&CK techniques  
- Visualize results in **Kibana dashboards**  

---

## 🖼️ Architecture  
![System Architecture](assets/diagram.png)  

---

## ⚙️ Setup Instructions  

### 1. Install Elastic Stack on Host (Windows 11)  
1. Download [Elasticsearch](https://www.elastic.co/downloads/elasticsearch) and [Kibana](https://www.elastic.co/downloads/kibana)  
2. Configure `elasticsearch.yml`  

```yaml
node.name: node-1
cluster.name: winlogs-cluster
network.host: 0.0.0.0
discovery.type: single-node
```

3. Configure kibana.yml

server.host: "0.0.0.0"
elasticsearch.hosts: ["http://localhost:9200"]


4. Start Elasticsearch and Kibana

### 2. Deploy Windows Server 2019 VM in VirtualBox

1. Install Windows Server 2019 ISO

2. Network setup:

Adapter 1: Host-only (host ↔ VM communication)

Adapter 2 (optional): NAT (internet access)

3. Assign static IP (same subnet as VirtualBox host adapter)

4. Verify connectivity

ping <host-ip>
ping <vm-ip>

3. Install Sysmon on the VM

Download Sysmon from Microsoft Sysinternals

Download trusted XML config (e.g. SwiftOnSecurity sysmon-config
)

Install Sysmon

sysmon.exe -accepteula -i sysmon.xml


Verify logs in Event Viewer:
Applications and Services Logs → Microsoft → Windows → Sysmon/Operational

### 4. Configure Fleet and Elastic Agent

In Kibana → Fleet → Settings, add Fleet Server

- Host URL = Host’s VirtualBox Ethernet IP

- Output = Elasticsearch endpoint (http://<host-ip>:9200)

Copy Fleet Server enrollment command → run on host

Add Elastic Agent → copy command → run on Windows Server VM

Verify VM appears in Kibana → Fleet → Agents

### 5. Add Integrations

Windows Integration (system + security logs)

Elastic Defend (EDR) (endpoint monitoring)

### 6. Enable Security Features

In Kibana Security app → enable SIEM detections & alerts

Configure detection rules

### 7. Simulate Attacks with Atomic Red Team

Install Atomic Red Team
 on Windows Server VM

Run MITRE ATT&CK simulations

Invoke-AtomicTest T1059.001


Confirm logs and alerts in Kibana

### 8. Visualize in Kibana

Discover → raw logs (Sysmon, Elastic Defend, Windows events)

Dashboards → curated Elastic Security dashboards

Detections → alerts triggered by Atomic Red Team simulations

---

## ✅ Results & Conclusion

Successfully ingested Windows Event Logs with Elastic Agent + Sysmon

Elastic Defend detected simulated ATT&CK techniques

Kibana dashboards enabled interactive visualization and analysis

Conclusion: Elastic Stack provides a solid baseline for Windows log monitoring and security training.

---

## 🚀 Future Work

Expand to multi-node Elastic clusters

Add custom dashboards for specific log types

Integrate Elastic with other open-source tools (Splunk, Wazuh, Graylog)

path.logs: C:/elastic/logs
path.data: C:/elastic/data
