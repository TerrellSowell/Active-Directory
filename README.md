# Active Directory
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>

## Objective


This lab demonstrates how to install and configure Active Directory using Azure. I used two virtual machines on Azure that are on the same virtual network. One VM I installed Active Directory and configured it to be the Domain Controller and the other VM I used as a client. Then, I configured the Active Directory to allow the client to join the domain as well as creating user accounts using a Powershell script. This Active Directoy Lab project aimed to establish a controlled environment for simulating, troubleshooting,  both System Administrator and I.T. helpdesk functionality.

### Skills Learned

- Advanced understanding of Active Directory concepts and Domain Controller application.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools and Environement Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Powershell
- Windows Server 2022
- Windows 10

## Steps
* **Step 1: Set up resources in Azure**<p>
Not shown in the photos belows are the setup of two virtual machines in Azure 
  - Create the Domain Controller VM (Windows Server 2022) named “DC-1”
      - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time.
  - Set Domain Controller’s NIC Private IP address to be static (shown in photos below)
  - Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
  - Ensure that both VMs are in the same Vnet
![set Domain Controller's NIC Private IP to static](https://github.com/TerrellSowell/Active-Directory/assets/161978506/c08b1a43-da8e-466a-a172-cf58eeb76af4)
![static 2](https://github.com/TerrellSowell/Active-Directory/assets/161978506/06d6d23e-7791-4724-90b7-7d391fcf4a15)
![dynamic](https://github.com/TerrellSowell/Active-Directory/assets/161978506/be559454-a23c-4ac5-b901-98abf1f9d4d1)
![static](https://github.com/TerrellSowell/Active-Directory/assets/161978506/c50043d7-ed64-4c17-bca9-d6e405fac0ba)<p>

* **Step 2: Ensure Connectivity between the client and Domain Controller**<p>
  - Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping) you will get a timeout due to the Domain's firewall settings.(Not shown in photos)
  - Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
  - Check back at Client-1 to see the ping succeed
![firewall](https://github.com/TerrellSowell/Active-Directory/assets/161978506/7c39ef29-551c-4269-8d99-59d5af4abec0)
![enable rule inbound](https://github.com/TerrellSowell/Active-Directory/assets/161978506/8375af5e-e99f-4564-b05f-ce05dc17d3db)
![successful reply](https://github.com/TerrellSowell/Active-Directory/assets/161978506/3dd104dd-cb4c-4c1a-aa2c-41e231b293a5)<p>

* **Step 3: Install Active Directory**<p>
  - Login to DC-1 and install Active Directory Domain Services
  - Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
  - Restart and then log back into DC-1 as user: mydomain.com\labuser













