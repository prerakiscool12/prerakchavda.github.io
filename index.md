---
layout: default
---


# Azure SIEM Deployment

## Objective:

This project focuses on setting up a Security Information and Event Management (SIEM) system using Azure Sentinel to collect, monitor, and analyze security logs from a virtual machine (VM). The setup is designed to simulate a real-world SOC (Security Operations Center) environment, where logs are generated from a running VM and forwarded to Azure Sentinel for real-time security monitoring and alerting.

## Project Components:

**Azure Sentinel** – For SIEM functionality and security log analysis.

**Azure Virtual Machine** – Simulated a real-world system generating logs for monitoring.

**Custom Detection Rules** – Created to alert on specific activities like successful logins.

**Azure Logic Apps** – For automating email notifications when alerts are triggered.

## Setup Steps:

### **1. Deploying an Azure Virtual Machine (VM)**

#### Log in to the Azure Portal/Create account for Azure:
      Go to Azure Portal if you have a account: https://portal.azure.com/#home
      Or create a new Azure account and use the free credit to help deploy a new virtual machine.

  #### Create a new virtual machine:
        Navigate to “Virtual Machines” in the left-hand menu and click “Create”.
        Choose your Subscription and Resource Group.
        Set the Region (I chose East US).
        Select an Image (e.g., Ubuntu 20.04 or Windows Server).
        Choose the VM size (I used Standard_B1ms for cost efficiency).

   #### Configure networking:
        Ensure inbound port rules allow access to port 3389 for RDP (if using Windows) or port 22 for SSH (if using Linux).

 #### Storage settings:
        Select the storage size, and leave the rest of the options as default.

  #### Create the VM:
        Once the VM is created, connect to it using SSH or RDP.

 #### Install Sysmon (optional, if using Windows VM):
        Sysmon generates detailed system event logs. Install using: sysmon -accepteula -i sysmonconfig.xml 

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Large image

![Branching](https://guides.github.com/activities/hello-world/branching.png)


### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
