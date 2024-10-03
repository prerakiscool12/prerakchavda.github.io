---
layout: default
---


# Azure SIEM Deployment

## Objective:

This project focuses on setting up a Security Information and Event Management (SIEM) system using Azure Sentinel to collect, monitor, and analyze security logs from a virtual machine (VM). The setup is designed to simulate a real-world SOC (Security Operations Center) environment, where logs are generated from a running VM and forwarded to Azure Sentinel for real-time security monitoring and alerting. This is not meant to be a guide, just a walkthrough of what I did for this project.

## Project Components:

**Azure Sentinel** – For SIEM functionality and security log analysis.

**Azure Virtual Machine** – Simulated a real-world system generating logs for monitoring.

**Custom Detection Rules** – Created to alert on specific activities like successful logins.

**Azure Logic Apps** – For automating email notifications when alerts are triggered.

## Setup Steps:

### **1. Deploying an Azure Virtual Machine (VM)**

**Log in to the Azure Portal/Create account for Azure:**
      Go to Azure Portal if you have a account: https://portal.azure.com/#home
      Or create a new Azure account and use the free credit to help deploy a new virtual machine.

**Create a new virtual machine:**
        Navigate to “Virtual Machines” in the left-hand menu and click “Create”.
        Choose your Subscription and Resource Group.
        Set the Region (I chose East US).
        Select an Image (e.g., Ubuntu 20.04 or Windows Server).
        Choose the VM size (I used Standard_B1ms for cost efficiency).

**Configure networking:**
        Ensure inbound port rules allow access to port 3389 for RDP (if using Windows) or port 22 for SSH (if using Linux).

**Storage settings:**
        Select the storage size, and leave the rest of the options as default.

**Create the VM:**
        Once the VM is created, connect to it using SSH or RDP.
**
Install Sysmon (optional, if using Windows VM):**
        Sysmon generates detailed system event logs. Install using: 
```ruby
# sysmon -accepteula -i sysmonconfig.xml 
```


### **2. Configuring Azure Sentinel**

**Enable Azure Sentinel:**
        In the Azure portal, navigate to Azure Sentinel.
        Click on Create and choose your existing Log Analytics Workspace or create a new one.

**Link Azure Sentinel to your VM:**
        Go to Data connectors in Azure Sentinel and look for Windows Security Events or Syslog (if using Linux).
        Enable the relevant connector by configuring the VM as the source.
      
        For Windows, you’ll need to install the Log Analytics Agent on the VM to collect security logs:
            Download the agent from the Log Analytics Workspace -> Advanced settings -> Windows Agents.
            After installation, connect the agent to your Log Analytics workspace by entering the workspace ID and key.

### **3. Setting Up Log Forwarding from VM to Azure Sentinel**

**For Windows VM:**

    Install the Log Analytics agent on the VM.
    Configure the agent with your Workspace ID and Primary Key from the Azure portal.

**For Linux VM:**

    Use the omsagent to send logs:

    bash

    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <PrimaryKey>

    This will forward Syslog and custom logs to Azure Sentinel.

### **4. Creating Custom Detection Rules in Azure Sentinel**

Azure Sentinel allows for the creation of custom rules to detect specific security events. I created a custom rule to detect successful login attempts.


**Navigate to Detection Rules:**
        In Azure Sentinel, click on Analytics -> Create Rule.

**Define your custom rule:**
        Name: Successful Login Detection.
        Tactic: Initial Access.
        Severity: Medium (or High, depending on your preference).

**Rule Logic:**
        Use Kusto Query Language (KQL) to define the rule. This example detects successful login events from Windows event logs:

        kql

        SecurityEvent
        | where EventID == 4624
        | where LogonType == 2
        | project Account, TimeGenerated, LogonType, Computer

        This query filters for Windows Event ID 4624, which indicates successful logons.

**Set up Alerts:**
        Configure the rule to trigger an alert whenever a match is found.
        You can set the rule to run every 5 minutes for real-time monitoring.

### **5. Automating Incident Response with Azure Logic Apps**

To automate incident response, I integrated Azure Logic Apps to send email notifications when a security event is detected.


**Create a new Logic App:**
        In the Azure portal, search for Logic Apps and create a new Logic App.

**Trigger the Logic App with an Alert:**
        Use the Azure Sentinel Alert as the trigger for your Logic App.
        Set it to trigger when your custom rule for successful logins is met.

**Action – Send Email Notification:**
        In the Logic App workflow designer, add an action to send an email via Outlook or SMTP.
        Customize the email body with details from the alert, such as:
            User who logged in.
            Time of the event.
            IP address (if available).
