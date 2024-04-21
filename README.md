# Active Directory
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>

## Objective


This lab demonstrates how to install and configure Active Directory using Azure. I used two virtual machines on Azure that are on the same virtual network. One VM I installed Active Directory and configured it to be the Domain Controller and the other VM I used as a client. Then, I configured the Active Directory to allow the client to join the domain as well as creating user accounts using a Powershell script. This Active Directoy Lab project aimed to establish a controlled environment for simulating, troubleshooting,  both System Administrator and I.T. helpdesk functionality in a later lab.

### Skills Learned<h2>
- Advanced understanding of Active Directory concepts and Domain Controller application.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols (DNS) and security procedures (Firewalls).
- Development of critical thinking and problem-solving skills in Microsoft Applications.

### Environment and Tools Used<h2>
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Windows Server 2022
- Windows 10

## Steps
* **Step 1: Set up resources in Azure**<p>
Go to your Domain Controller virtual machine in Azure and go to Networking then go to the link listed next to Network Interface. Head to IP Configurations under settings, go to the ipconfig link to open up a window to toggle the IP configuration and allocation to Static.
  - Create the Domain Controller VM (Windows Server 2022) named “DC-1”
  - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time.
  - Set Domain Controller’s NIC Private IP address to be static (shown in photos below)
  - Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created prior.
  - Ensure that both VMs are in the same Vnet
![set Domain Controller's NIC Private IP to static](https://github.com/TerrellSowell/Active-Directory/assets/161978506/c08b1a43-da8e-466a-a172-cf58eeb76af4)
![static 2](https://github.com/TerrellSowell/Active-Directory/assets/161978506/06d6d23e-7791-4724-90b7-7d391fcf4a15)
![dynamic](https://github.com/TerrellSowell/Active-Directory/assets/161978506/be559454-a23c-4ac5-b901-98abf1f9d4d1)
![static](https://github.com/TerrellSowell/Active-Directory/assets/161978506/c50043d7-ed64-4c17-bca9-d6e405fac0ba)<p>

* **Step 2: Ensure Connectivity between the client and Domain Controller**<p>
Logging into the Domain Controller VM, go to the Windows Defender Firewall with Advanced Security. Head to the Inbound Rules and enable the rules under the protocol ICMPv4, specifically Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)
  - Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) you will get a timeout due to the Domain's firewall settings.(Not shown in photos)
  - Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
  - Check back at Client-1 to see the ping succeed
![firewall](https://github.com/TerrellSowell/Active-Directory/assets/161978506/7c39ef29-551c-4269-8d99-59d5af4abec0)
![enable rule inbound](https://github.com/TerrellSowell/Active-Directory/assets/161978506/8375af5e-e99f-4564-b05f-ce05dc17d3db)
![successful reply](https://github.com/TerrellSowell/Active-Directory/assets/161978506/3dd104dd-cb4c-4c1a-aa2c-41e231b293a5)<p>

* **Step 3: Install Active Directory on Domain Controller**<p>
In your Domain Controller VM, go to the Server Manager Dashboard and click on Add Roles and Features. Go through the installation process and upon getting to Server Roles, make sure to check the box for Active Directory Domain Services. Once installed, I now have to promote the server into a domain controller. To do so, you may notice a warning notification on the top right where the flag icon is. Click on that flag and click Promote this server to a domain controller. Click on Add a new forest and specify a domain name. For this tutorial, I named the domain TerrellSowell.com, created the password, and proceed with the install. Noted, you will be automatically signed out, re-log in through Remote Desktop, and installation is fully completed! 
  - Login to DC-1 and install Active Directory Domain Services
  - Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
  - Restart and then log back into DC-1 as user: mydomain.com\labuser
![install AD](https://github.com/TerrellSowell/Active-Directory/assets/161978506/56cb26ea-9d42-4733-8004-212e455da7b0)
![install AD2](https://github.com/TerrellSowell/Active-Directory/assets/161978506/9529ee16-1e84-4c4a-a9ae-6286adb01233)
![installAD](https://github.com/TerrellSowell/Active-Directory/assets/161978506/8945b953-e81d-453a-80c4-cf0f6090fcef)
![installAD3](https://github.com/TerrellSowell/Active-Directory/assets/161978506/29b99aa4-d67e-46b8-a0ed-c3059e38dbe6)
![intallAD4](https://github.com/TerrellSowell/Active-Directory/assets/161978506/80822df9-efa5-4af4-9af6-d5930997b6b7)
* **Important Log In Note:** When logging back in to the domain controller VM through Remote Desktop Connection, it is important to log in with the context of the domain.
Type out the domain path and then the name of the user. For example: mydomain.com/labuser.

* **Step 4: Create an Admin and Normal User Account in Active Directory**<p>
OUs act like folders that hold information, privileges, and login access of users in the active directory.
 - In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
 - Create a new OU named “_ADMINS”
 - Create a new employee named “Lebron James” (same password) with the username of “James_admin”
 - Add James_admin to the “Domain Admins” Security Group
 - Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\james_admin”
 - User james_admin as your admin account from now on<p>
 ![created new admin](https://github.com/TerrellSowell/Active-Directory/assets/161978506/39a149c2-0de6-4efa-b39d-9a6cf27b6e63)<p>

* **The photo shown below is granting James admin privileges. Using the Security Group, right click on the user and open their Properties. Click "Member Of" tap then click "Add" to apply to the appropriate security group.**
![add admin permissions](https://github.com/TerrellSowell/Active-Directory/assets/161978506/e8c5e828-d80d-40fa-aebe-ec8231bf838d)<p>

* **Step 5: Join Client-1 to your domain (TerrellSowell.com)** <p>
First, configure the Domain Name System (DNS) server. Go to the Domain Controller VM in the Azure Portal and go to "Networking" then go to the link listed next to Network Interface. Head to "DNS Servers" under settings, and set the DNS Server to Custom. Then, enter the domain controller's private IP address and save the changes. Restart the client VM in order to ensure the DNS changes are saved. Next, in the System menu of the client VM, click on "Rename this PC (advanced)" and Change. Enter the domain and necessary credentials in order to let the client join the domain (logging in as james_admin). It is important to note that the login credentials have to be input within the context of the domain path (TerrellSowell.com\james_admin).
  - From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
  - From the Azure Portal, restart Client-1
  - Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
  - Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
![add pc to domain](https://github.com/TerrellSowell/Active-Directory/assets/161978506/43771f7d-3c38-41d4-963f-1d9de2850f26)
![DNS for client1](https://github.com/TerrellSowell/Active-Directory/assets/161978506/50c1a467-109b-491a-a280-5554985143f0)
![DNS2](https://github.com/TerrellSowell/Active-Directory/assets/161978506/01dfbc36-5c61-4f99-8951-3ecec990f0db)
![client1 added to DC](https://github.com/TerrellSowell/Active-Directory/assets/161978506/491fdade-e95e-4ad8-8e80-c2515171f65e)
![remote in ](https://github.com/TerrellSowell/Active-Directory/assets/161978506/9c777c97-22d2-43c0-8c6d-66401f09bb94)
![adding remote users](https://github.com/TerrellSowell/Active-Directory/assets/161978506/44ea79ca-c2c6-4ef2-ad88-3126cc0cd5a5)
![remote users that can log in](https://github.com/TerrellSowell/Active-Directory/assets/161978506/9568c0d9-99f1-4d4e-aabc-8acf5020b922)

























