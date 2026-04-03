# Active Directory Lab/Project

In this lab, I demonstrate how to:  

- Apply real-world IT Helpdesk and System Administration skills, including **basic networking**  
- Deploy a Windows Server on AWS to manage IT resources for a demo company  
- Configure **TCP/IP** settings for the server  
- Set up **DNS** and **DHCP** services as part of the domain controller  
- Promote the server to a Domain Controller  
- Create and manage users and groups  
- Reset passwords and assign permissions  
- Enable Remote Desktop access to the virtual machine hosted on AWS

---

## Tech Stack

- AWS EC2 (Windows Server)  
- Windows Server  
- Active Directory Domain Services (AD DS)  
- Remote Desktop Protocol (RDP)  
- PowerShell (for automation)

---

## Step 1: Launch a Windows Server on AWS

- I launched an EC2 instance on AWS using the **Windows Server 2025** AMI as the operating system.  
  ![AWS Windows Server 2025](Screenshots/aws_windows_server_2025.png)

- I then downloaded a `.pem` key pair to log into my EC2 instance.  
  ![Create Key Pair](Screenshots/create_key_pair.png)

- I then created a **Security Group** to enable **RDP** access. I allowed inbound **TCP** traffic on **port 3389** from my **IP address** only.  
  ![Security Group](Screenshots/security_group.png)

- I made sure to leave the **Auto-assign public IP** option enabled. This setting automatically allocates an IP address to your instance using AWS’s DHCP service.  
  ![Auto Assign IP](Screenshots/auto_assign_ip.png)

- AWS assigned a **public IP** to my instance via **DHCP** by default. I also experimented with attaching an **Elastic IP** to give the server a permanent address." 
  ![EC2 ID](Screenshots/ec2_id.png)  
  ![IP Address](Screenshots/ip_address.png)

- I ensured that my instance passed all status checks and was up and running.  
  ![Running Instance](Screenshots/running_instance.png)

- AWS provided a public **DNS** link for my EC2 instance, which I used to connect without needing to remember the **IP**. 
  Example: `ec2-13-246-3-184.af-south-1.compute.amazonaws.com`  
  This identifies the server when connecting via RDP.  
  ![Public DNS](Screenshots/public_dns.png)

- Now I’m ready to connect via RDP. I selected the EC2 instance and clicked **Connect**, then chose the **RDP client** option.  
  ![RDP Client](Screenshots/rdp_client.png)

- I uploaded the `.pem` key pair file to decrypt the Administrator password.  
  ![Key Pair](Screenshots/key_pair.png)  
  ![Password](Screenshots/password.png)

- I downloaded the Remote Desktop file and double-clicked it to open.  
  ![Remote Desktop File](Screenshots/remote_desktop_file.png)

- When connecting to the server, Windows may display a warning about an unrecognized security certificate. EC2 instances use a self-signed certificate. Since I created the server, I proceeded to connect.  
  ![Windows Error](Screenshots/windows_error.png)

- At this point, I have successfully connected to the instance via Remote Desktop (RDP).  
  ![Server Desktop](Screenshots/server_desktop.png)

---

## Step 2: Installing Active Directory Domain Services (AD DS)

- I clicked **Start** and opened **Server Manager**. In **Server Manager**, I clicked **Add Roles and Features**.  
  ![Server Manager Dashboard](Screenshots/server_manager_dashboard.png)

- Then I chose **Role-based or feature-based installation**.  
  ![Role Based Installation](Screenshots/role_based_installation.png)

- I selected my EC2 instance from the server pool.  
  ![Server Select](Screenshots/server_select.png)

- I checked **Active Directory Domain Services** and clicked **Next** to install.  
  ![Select AD](Screenshots/select_ad.png)

- I left the default features selected and proceeded with the installation. 
  ![Features](Screenshots/features.png)

- I let the installation process complete.  
  ![AD DS Installation](Screenshots/ad_ds_installation.png)

- After installation, a notification appeared to **Promote this server to a domain controller**.  
  ![Promote DC](Screenshots/promote_dc.png)

---

## Step 3: Promote Server to Domain Controller

- I clicked **Promote this server to a domain controller** from the notification in Server Manager.  

- I chose **Add a new forest** and entered my root domain name: `tebogo.lab`.  

- I set the **DSRM password** for Directory Services Restore Mode.  

- I ensured **DNS** and **Global Catalog** were enabled.  

- I clicked **Next** and then **Install**. The server restarted automatically.

- I also tested promotion using **PowerShell** to deploy the domain controller via command line.  
  ![Powershell](Screenshots/powershell.png)

---

## Step 4: Create an Organizational Unit (OU)

- After the server restarted, I logged in as **Administrator**.  

- I opened **Active Directory Users and Computers**.  
  ![Users And Computers](Screenshots/users_and_computers.png)

- I right-clicked my domain (`tebogo.lab`) and selected **New → Organizational Unit**.  
  ![OU](Screenshots/ou.png)

- I named the OU `TebogoLabUsers`.  
  ![Tebogo Lab Users](Screenshots/tebogo_lab_users.png)

- I created this OU to organize the users and groups in my domain for easier management.

---

## Step 5: Creating Users

- I navigated to the OU `TebogoLabUsers` in **Active Directory Users and Computers**.  

- I right-clicked the OU and selected **New → User**.  
  ![New User](Screenshots/new_user.png)

- I created a user called `John Doe` with the username `jdoe`.  
  ![John Doe](Screenshots/john_doe.png)

- I set an initial password and enabled **User must change password at next logon**.  
  ![Password Next Logon](Screenshots/password_next_logon.png)

- I created additional users following the same steps.  
  ![More Users](Screenshots/more_users.png)

---

## Step 6: Creating Groups

- I navigated to the OU `TebogoLabUsers`.  

- I right-clicked the OU and selected **New → Group**.  

- I created a group called `HR`, set the scope to **Global**, and type to **Security**.  
  ![HR Group](Screenshots/hr_group.png)

- I added users to this group to assign permissions efficiently. For example, Jane Van Wyk was added to HR. Her membership is visible in **Properties → Member Of**.  
  ![Jane HR](Screenshots/jane_hr.png)

---

## Step 7: Assign Remote Desktop Permissions

- I navigated to each user in **Active Directory Users and Computers**.  

- I added users to the **Remote Desktop Users** group to allow RDP access. Otherwise, they could not log on.  
  ![Remote Desktop Users](Screenshots/remote_desktop_users.png)

- Users can now log in to the AWS EC2 instance using Remote Desktop.

---

## Errors and Challenges

During this lab, I faced some challenges:

- **“Install” button greyed out for AD DS**  
  - Resolution: Ensured all required features and roles were selected, then clicked **Next** through the wizard to install.

- **DNS delegation warning**  
  - Resolution: I saw a DNS delegation warning after promoting the server. Since this is a new forest, I acknowledged it and continued.

- **Error creating user: name already in use**  
  - Resolution: Used unique usernames like `jdoe` to avoid duplicates.

- **Unable to log in via RDP with non-admin users**  
  - Resolution: Added users to the **Remote Desktop Users** group and updated Group Policy.

- **Password issues for new users**  
  - Resolution: Reset passwords and enabled **User must change password at next logon**.

- **Understanding AD terminology**  
  - Resolution: Studied terms like **forest**, **domain**, **NetBIOS name**, and **DSRM** and applied them correctly.

- **Automating AD DS deployment with PowerShell**  
  - Resolution: Used Windows-provided PowerShell script to deploy the domain controller automatically:  

```powershell
Import-Module ADDSDeployment

Install-ADDSForest `
    -CreateDnsDelegation:$false `
    -DatabasePath "C:\Windows\NTDS" `
    -DomainMode "Win2025" `
    -DomainName "tebogo.lab" `
    -DomainNetbiosName "TEBOGO" `
    -ForestMode "Win2025" `
    -InstallDns:$true `
    -LogPath "C:\Windows\NTDS" `
    -NoRebootOnCompletion:$false `
    -SysvolPath "C:\Windows\SYSVOL" `
    -Force:$true