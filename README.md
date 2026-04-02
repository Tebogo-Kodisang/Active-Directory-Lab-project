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

## Tech Stack

- AWS EC2 (Windows Server)  
- Windows Server  
- Active Directory Domain Services (AD DS)  
- Remote Desktop Protocol (RDP)  
- PowerShell (for automation)

## Step 1: Launch a Windows Server on AWS

- I launched an EC2 instance on AWS using the **Windows Server 2025** AMI as the operating system.  
![AWS Windows Server 2025](Screenshots/aws_windows_server_2025.png)

- I then downloaded a `.pem` key pair so that I could later connect via RDP (Remote Desktop Protocol).

- AWS then assigns an instance ID and a public IP address to your instance (**basic TCP/IP setup**)  
![EC2 ID](Screenshots/ec2_id.png)  
![IP Address](Screenshots/ip_address.png)

- The server can use DHCP or a static IP, demonstrating basic networking skills.

- I then ensured that my instance passed all status checks and was up and running.  
![Running Instance](Screenshots/running_instance.png)

- AWS provides a public DNS link for the EC2 instance. This allows you to connect without needing to remember the IP address. (**DNS resolution for RDP access**) 
Example: `ec2-13-246-3-184.af-south-1.compute.amazonaws.com`  
This is used to identify the server when using your PC to connect via RDP.  
![Public DNS](Screenshots/public_dns.png)

- Now you are ready to connect via RDP. Select the **RDP client** tab on your instance.  
![RDP Client](Screenshots/rdp_client.png)
 
- I uploaded the `.pem` key pair file to decrypt the Administrator password.  
![Key Pair](Screenshots/key_pair.png)  
![Password](Screenshots/password.png)

- When connecting to your server, Windows may show a warning. This is because AWS uses a self-signed certificate instead of one from a big trusted authority.  
Since I created the server, it is safe to proceed and connect.  
![Windows Error](Screenshots/windows_error.png)

- At this point, I have successfully connected to the instance via Remote Desktop (RDP).  
![Server Desktop](Screenshots/server_desktop.png)