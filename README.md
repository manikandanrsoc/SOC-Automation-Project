# SOC-Automation-Project

## Objective
To build and automate a Security Operations Center (SOC) environment using various security tools including Wazuh, TheHive, and Shuffle, and to integrate them to handle alerts and incidents effectively.

### Skills Learned
- Cloud infrastructure setup and management
- Installation and configuration of security tools (Wazuh, TheHive, Sysmon, Shuffle)
- Generating and analyzing security telemetry
- SOAR (Security Orchestration, Automation, and Response) implementation
- Incident response and alert management
- Automation of security processes

### Tools Used
- Wazuh: Security monitoring tool
- TheHive: Incident response platform
- Shuffle: SOAR platform
- Sysmon: System activity monitoring tool
- Virtual Machines (VMs): For hosting different components
- Cloud Platform: For deploying and managing infrastructure

## Steps
Step 1: Draw a Logical Diagram
 
 - Objective: To Visualize the architecture of the SOC enviroment.
 - Tools: Draw.io or any diagramming tool.
 - Details: Include components like Wazuh Server, TheHive Server, Windows 10 VM with Sysmon, and the integration points between them.

Step 2: Install the Components and Set Up in the Cloud

  - Objective: To deploy and install necessary applications and virtual machines.
  - Tools: Cloud platform AWS.
  Steps:
       
       1. Deploy Virtual Machines: Create VMs for Wazuh Server, TheHive Server, and a Windows 10 machine.
       2. Install Wazuh Server: Follow Wazuh installation documentation to set up the server.
       3. Install TheHive Server: Follow TheHive installation documentation for the setup.
       4. Install Windows 10 with Sysmon: Configure Sysmon on Windows 10 for detailed event logging.

Step 3: Configure the Servers and Endpoint

  - Objective: To configure the installed components to communicate with each other.
  - Tools: Configuration files, SSH, RDP.
  Steps:

       1. Configure Wazuh Agent on Windows 10: Ensure the Wazuh agent is installed and communicating with the Wazuh Server.
       2. Integrate TheHive with Wazuh: Configure Wazuh to send alerts to TheHive.
       3. Set up Sysmon: Configure Sysmon rules to log specific events.

Step 4: Generate Telemetry Related to Mimikatz

  - Objective: To simulate a security incident by generating telemetry.
  - Tools: Mimikatz, Sysmon.
    Steps:

       1. Deploy Mimikatz on Windows 10: Execute Mimikatz to generate security events.
       2. Monitor Logs: Ensure Sysmon logs the events and Wazuh captures them.

Step 5: Set Up SOAR and Integrate Everything Together

  - Objective: To automate alert handling and incident response.
  - Tools: Shuffle, TheHive, Wazuh.
    Steps:

       1. Install Shuffle: Follow installation documentation for Shuffle.
       2. Integrate Wazuh with Shuffle: Configure Shuffle to receive alerts from Wazuh.
       3. Integrate Shuffle with TheHive: Set up workflows in Shuffle to create cases in TheHive based on alerts.
       4. Automate Email Notifications: Configure Shuffle to send email notifications to analysts.
       5. Test the Integration: Generate test alerts and verify they are processed by the SOAR platform, cases are created in TheHive, and emails are sent.
   


# SOC Automation Project 3.0 — Wazuh + Shuffle (SOAR) + TheHive (Case Mgmt) + VirusTotal Enrichment

A hands-on SOC automation lab that simulates a real incident workflow: endpoint telemetry triggers a **custom detection** in **Wazuh**, the alert is forwarded to **Shuffle (SOAR)** via webhook, enriched with **VirusTotal**, then **creates an alert/case in TheHive** and **emails the SOC analyst** for action.

This project is designed to demonstrate end-to-end SOC engineering skills: detection → orchestration → enrichment → case management → analyst notification.  
(High-level lab concept: Wazuh as SIEM/XDR, Shuffle as SOAR, TheHive as case management. :contentReference[oaicite:0]{index=0})

---

## What I Built (Outcome)

### Automated Workflow
1. **Windows endpoint** generates security telemetry (Sysmon / Windows logs).
2. **Wazuh Manager** detects a suspicious activity (custom rule for Mimikatz-style behavior/strings/telemetry).
3. Wazuh **forwards only the targeted rule** to **Shuffle** using an **integration tag** in `ossec.conf` and a **Shuffle webhook**. :contentReference[oaicite:1]{index=1} :contentReference[oaicite:2]{index=2}
4. **Shuffle parses (REX) the SHA-256 hash** from the alert payload and prepares it for enrichment. :contentReference[oaicite:3]{index=3}
5. Shuffle enriches the hash using **VirusTotal API v3** and captures **reputation/detections** (example: malicious count). :contentReference[oaicite:4]{index=4} :contentReference[oaicite:5]{index=5}
6. Shuffle sends the incident details to **TheHive** for **case management**. :contentReference[oaicite:6]{index=6}
7. Shuffle also **emails the analyst** with alert + enrichment context to start triage. :contentReference[oaicite:7]{index=7}

---

## Key Skills Demonstrated

- **Detection Engineering:** tuned Wazuh to ingest endpoint telemetry and fire a custom alert (Sysmon + Windows Event pipeline).
- **SOAR Orchestration:** built a webhook-driven Shuffle workflow and controlled routing to reduce noise by forwarding only a specific rule ID. :contentReference[oaicite:8]{index=8} :contentReference[oaicite:9]{index=9}
- **Threat Intel Enrichment:** integrated VirusTotal hash reputation checks via API v3 and extracted meaningful fields for analyst context. :contentReference[oaicite:10]{index=10} :contentReference[oaicite:11]{index=11}
- **Case Management:** structured alerts in TheHive (org + users/service account approach) to mimic real SOC ops. :contentReference[oaicite:12]{index=12} :contentReference[oaicite:13]{index=13}
- **Troubleshooting & Tooling:** validated integrations end-to-end and fixed API/connector issues (e.g., correcting VirusTotal endpoint format when a workflow action returned 404). :contentReference[oaicite:14]{index=14} :contentReference[oaicite:15]{index=15}

---

## Architecture

**Windows 10 Endpoint (Sysmon/Logs)**  
→ **Wazuh Manager (Detection + Alerting)**  
→ **Shuffle Webhook (SOAR Trigger)** :contentReference[oaicite:16]{index=16}  
→ **Shuffle Apps**
- REX: extract SHA-256 hash :contentReference[oaicite:17]{index=17}  
- VirusTotal: hash reputation enrichment :contentReference[oaicite:18]{index=18}  
- TheHive: create alert/case :contentReference[oaicite:19]{index=19}  
- Email: notify analyst :contentReference[oaicite:20]{index=20}

---

## Step-by-Step (How I Implemented It)

### 1) Create the Workflow in Shuffle
- Created a new Shuffle workflow and added a **Webhook trigger** as the workflow starter. :contentReference[oaicite:21]{index=21}
- Copied the generated webhook URL to use in Wazuh integration settings. :contentReference[oaicite:22]{index=22}

### 2) Configure Wazuh → Shuffle Integration
- Edited Wazuh manager configuration (`ossec.conf`) and added an **integration** block pointing to the Shuffle webhook URL. :contentReference[oaicite:23]{index=23} :contentReference[oaicite:24]{index=24}
- Adjusted filtering so only the intended alert is forwarded:
  - Switched from forwarding by **level** to forwarding by **rule_id** (targeted custom rule). :contentReference[oaicite:25]{index=25} :contentReference[oaicite:26]{index=26}
- Restarted the Wazuh manager service to apply changes. :contentReference[oaicite:27]{index=27}

### 3) Generate Telemetry & Validate Forwarding
- Regenerated suspicious telemetry on the Windows endpoint (to trigger the custom detection).
- Confirmed Shuffle receives the event through the webhook execution view. :contentReference[oaicite:28]{index=28}

### 4) Parse the SHA-256 Hash in Shuffle
- Added a parsing step (REX-style extraction) to pull the SHA-256 hash from the alert content. :contentReference[oaicite:29]{index=29}

### 5) Enrich with VirusTotal
- Activated the VirusTotal app in Shuffle and authenticated with VirusTotal API v3. :contentReference[oaicite:30]{index=30} :contentReference[oaicite:31]{index=31}
- Selected the **Hash Report** action and fed it the extracted hash. :contentReference[oaicite:32]{index=32} :contentReference[oaicite:33]{index=33}
- Troubleshot a 404 response by aligning the VirusTotal endpoint format to API v3 docs (files/{id}). :contentReference[oaicite:34]{index=34} :contentReference[oaicite:35]{index=35}
- Captured meaningful enrichment signal (example: `last_analysis_stats.malicious`). :contentReference[oaicite:36]{index=36} :contentReference[oaicite:37]{index=37}

### 6) Create the Case in TheHive
- Added TheHive app in Shuffle and used it to create an alert for case management. :contentReference[oaicite:38]{index=38} :contentReference[oaicite:39]{index=39}
- In TheHive, created an org and users (including a service account concept for integrations). :contentReference[oaicite:40]{index=40} :contentReference[oaicite:41]{index=41}

### 7) Notify the Analyst
- Added an email step so an analyst gets the alert + enrichment context to start triage. :contentReference[oaicite:42]{index=42}

---

## Repository Structure (Suggested)



Example below.

*Ref 1: Network Diagram*

