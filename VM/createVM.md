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
---
    <img width="1327" height="872" alt="Image" src="https://github.com/user-attachments/assets/cb1b0de8-dd5b-4d9b-b927-c0c757841c8e" />

---
<img width="1305" height="807" alt="Image" src="https://github.com/user-attachments/assets/4b867463-d098-459e-b869-0e3457d57086" />

4.  Click **`Review + create`**, then **`Create`**.

---

## Step 2: 💻 Create the Ubuntu VM

We will navigate the Virtual Machine creation wizard, focusing on standard configuration.

1.  In the Azure portal search bar, type **`Virtual machines`** and select the service.
2.  Click **`+ Create`**, then select **`Azure virtual machine`**.
3.  ### Basics Tab Configuration
    * **Project details:**
        * **Resource Group:** Select the one created in Step 1 (e.g., `UbuntuStandardGroup`).
    * **Instance details:**
        * **Virtual machine name:** (e.g., `MyStandardUbuntuVM`).
        * **Region:** Match the Resource Group region.
        * **Image:** Select **`Ubuntu Server 22.04 LTS`** (or your preferred version).
        * **Size:** Click **`See all sizes`** and select a size appropriate for your workload (e.g., `Standard_D2s_v5`). **Note:** This selection determines your cost.
    * **Administrator account:**
        * **Authentication type:** Choose **`SSH public key`** (recommended).
        * **Username:** (e.g., `ubuntuadmin`).
        * **SSH public key source:** Select **`Generate new key pair`**.
        * **Key pair name:** (e.g., `my-ssh-key`).
    * **Inbound port rules:**
        * **Public inbound ports:** Select **`Allow selected ports`**.
        * **Select inbound ports:** Choose **`SSH (22)`** from the dropdown.
4.  ### Review and Deployment
    * Click **`Review + create`**.
    * Review the configuration and the estimated hourly cost.
    * Click **`Create`**.
    * When prompted, click **`Download private key and create resource`**. **Save the downloaded `.pem` file securely!**

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

---

## Cost Management and Cleanup

When the VM is running (status is `Running`), it incurs compute charges.

| Action | Purpose |
| :--- | :--- |
| **Stop (Deallocate)** | Stops the running clock and halts compute charges. The VM disk/storage charges still apply. (Found on the VM Overview page). |
| **Delete** | Permanently removes the VM, its associated disk, network interface, and Public IP. |

Would you like the command to easily **delete** the entire Resource Group and all resources within it when you are finished?