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
  - Create the Domain Controller VM (Windows Server 2022) named “DC-1”
      - Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
  - Set Domain Controller’s NIC Private IP address to be static
  - Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
  - Ensure that both VMs are in the same Vnet
![set Domain Controller's NIC Private IP to static](https://github.com/TerrellSowell/Active-Directory/assets/161978506/c08b1a43-da8e-466a-a172-cf58eeb76af4)

