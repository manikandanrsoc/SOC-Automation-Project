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


Example below.

*Ref 1: Network Diagram*

