# 🚀 How to Launch an Ubuntu Server VM on Azure (Standard Deployment)

Step-by-step instructions for creating a Virtual Machine (VM) running Ubuntu Server using the Azure portal.

## Prerequisites

* An active Azure subscription.
* Access to the **Azure portal**.
* A local terminal/SSH client (e.g., Windows Terminal, PowerShell, macOS Terminal, or PuTTY).

---

## Step 1: 📂 Create a Resource Group

A Resource Group organizes your Azure resources.

1.  In the Azure portal search bar, type **`Resource groups`** and select the service.
2.  Click the **`+ Create`** button.
3.  Fill in the details:
    * **Subscription:** Select your subscription.
    * **Resource Group name:** Enter a descriptive name (e.g., `UbuntuStandardGroup`).
    * **Region:** Select the geographical region for your deployment.

<img width="1327" height="872" alt="Image" src="https://github.com/user-attachments/assets/cb1b0de8-dd5b-4d9b-b927-c0c757841c8e" />

---
<img width="1305" height="807" alt="Image" src="https://github.com/user-attachments/assets/4b867463-d098-459e-b869-0e3457d57086" />

4.  Click **`Review + create`**, then **`Create`**.

---

## Step 2: 💻 Create the Ubuntu VM

We will navigate the Virtual Machine creation wizard, focusing on standard configuration.

1.  In the Azure portal search bar, type **`Virtual machines`** and select the service.
2.  Click **`+ Create`**, then select **`virtual machine`**.
3.  ### Basics Tab Configuration
    * **Project details:**
        * **Resource Group:** Select the one created in Step 1 (e.g., `southGroup`).
    * **Instance details:**
        * **Virtual machine name:** (e.g., `MyStandardUbuntuVM`).
        * **Region:** Match the Resource Group region.
        * **Image:** Select **`Ubuntu Server 22.04 LTS`** (or your preferred version).
        
        <img width="931" height="683" alt="Image" src="https://github.com/user-attachments/assets/02a5e194-dcef-4ed6-9d15-c035f1ed1f6c" />

        * **Size:** Click **`See all sizes`** and select a size appropriate for your workload (e.g., `Standard_B2ats_v2 - 2 vcpus,`). **Note:** This selection determines your cost.
        <img width="962" height="472" alt="Image" src="https://github.com/user-attachments/assets/d9a16ea2-212f-412f-9d83-65b009f49297" />

    * **Administrator account:**
        * **Authentication type:** Choose **`azure_ssh`** (recommended).
        * **Username:** (e.g., `ubuntuadmin`).
        * **SSH public key source:** Select **`Generate new key pair`**.
        * **Key pair name:** (e.g., `my-ssh-key`).

    * **Inbound port rules:**
        * **Public inbound ports:** Select **`Allow selected ports`**.
        * **Select inbound ports:** Choose **`SSH (22)`** from the dropdown.
        <img width="965" height="563" alt="Image" src="https://github.com/user-attachments/assets/58161548-653f-4561-b091-f3c5423050aa" />

4.  ### Review and Deployment
    * Click **`Review + create`**.
    * Review the configuration and the estimated hourly cost.
    * Click **`Create`**.
    * When prompted, click **`Download private key and create resource`**. **Save the downloaded `.pem` file securely!**

    <img width="1377" height="591" alt="Image" src="https://github.com/user-attachments/assets/f371f7b9-b96e-4a85-9a04-6f259c7ecec9" />

---

## Step 3: 🔗 Connect to the Ubuntu VM (SSH)

You need the saved private key (`.pem` file) and the VM's Public IP address to connect.

1.  **Get the Public IP Address:**
    * Go to the VM's **Overview** page in the Azure portal and copy the **Public IP address**.
2.  **Connect via SSH:**
    * Open your terminal/command prompt.
    * **(Linux/macOS only):** Set permissions on the key file:
        ```bash
        chmod 400 /path/to/your/my-ssh-key.pem
        ```
    * **Establish the connection:**
        ```bash
        ssh -i /path/to/your/my-ssh-key.pem ubuntuadmin@<Your-Public-IP-Address>
        ```
        * Replace the key path and IP address with your actual values.
# Azure VM Port Configuration Guide

This document outlines the steps to add and configure an inbound port (8081) for an Azure Virtual Machine.

---

### 3. Update Inbound Rules in NSG
To allow traffic through the Azure platform, you must define a new security rule within the Network Security Group.

1.  **Locate the NSG:**
    * Log in to the [Azure Portal](https://portal.azure.com).
    * Search for **Virtual Machines** in the top search bar and select your VM.
    * In the left-hand menu, under the **Settings** section, click on **Networking**.
    * Click the link next to **Network Security Group** to open the configuration page.

2.  **Add the Inbound Rule:**
    * On the NSG page, select **Inbound security rules** from the left sidebar.
    * Click the **+ Add** button at the top.

3.  **Configure Rule Details:**
    Fill in the panel with the following values:

| Field | Value | Description |
| :--- | :--- | :--- |
| **Source** | Any | Allows traffic from any source. |
| **Source port ranges** | * | Allows any source port from the sender. |
| **Destination** | Any | Targets the VM associated with this NSG. |
| **Service** | Custom | Allows manual port definition. |
| **Destination port ranges**| **8081** | The specific port you want to open. |
| **Protocol** | TCP | Standard protocol for web/app traffic. |
| **Action** | Allow | Permits the incoming traffic. |
| **Priority** | 310 | Must be lower than the default Deny rule. |
| **Name** | Allow_8081_Inbound | A descriptive name for the rule. |

<img width="1863" height="848" alt="Image" src="https://github.com/user-attachments/assets/0cb6fd0c-2fbd-4f08-9da7-211e665c9900" />


4.  **Save:** Click **Add** and wait for the "Created security rule" notification.



---

### 4. Configure Guest OS Firewall
After the Azure platform allows the traffic, you must ensure the internal OS firewall is not blocking the port.

#### For Linux (Ubuntu/Debian)
```bash
sudo ufw allow 8081/tcp
sudo ufw reload
        
---

## Cost Management and Cleanup

When the VM is running (status is `Running`), it incurs compute charges.

| Action | Purpose |
| :--- | :--- |
| **Stop (Deallocate)** | Stops the running clock and halts compute charges. The VM disk/storage charges still apply. (Found on the VM Overview page). |
| **Delete** | Permanently removes the VM, its associated disk, network interface, and Public IP. |

Would you like the command to easily **delete** the entire Resource Group and all resources within it when you are finished?