# On-Premise-AD-Implementation-on-Azure

Requirements:
- Azure VM based on Windows Server 2019
- Install AD role
- Installing AD role will automatically install DNS Service

# What is a domain controller?
- Domain controller is a server on the network that centrally manages access for users, PCs and servers on the network. 
- It does this using AD. Active Directory is a database that organises your company's users and computers.
- DC also processes requests for authentication from users within a computer domain. 
- Domain controllers are most commonly used in Windows Active Directory (AD) domains but are also used with other types of identity management systems.

# What is a domain in Active Directory?
- An Active Directory domain is a collection of objects within a Microsoft Active Directory network. 
- An object can be a single user or a group or it can be a hardware component, such as a computer or printer. 
- Each domain holds a database containing object identity information. AD domain can manage multiple domains.

# What is a DNS Server?
- Domain Name System(DNS) is a server that is specifically used for matching website names (like google.com or microsoft.com) to their corresponding IP addresses.
- DNS Server contains a database of public IP addresses and their corresponding domain names.
- Think of a phone book where users can search for a person and then retrieve their phone number.

# Why is DNS server important to Active Directory?
- Active Directory uses uses DNS to find and resolve distinguished names into IP addresses.
- AD cannot work without DNS.

# Use Case: Install AD connect on on-premises AD environment and sync users to Azure AD environment
- Since I do not have a on premise environment, an Azure VM will act as an on-premise AD environment.
- Three VMs will be needed
- One VM will act as a Directory
- One VM will be the server where AD connect is installed
- One VM will be domain joined to the Directory where the Admin can log into.

# Create a VM and RDP into the VM to install AD DS on VM and ensure a Domain is in place. Call it <em> OnPremiseAD. </em>
- This VM will act as your on-premise Active Directory environment. 
- RDP into onpremAD 

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168497717-c7806db1-3191-4943-aa6a-d3b4ec4ff4ec.png" height="165%" width="165%" alt="Admin Units"/>
  
<p/>

- Navigate to <em> Add Roles and Feautures </em> 

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168498459-8b13dd70-103e-4828-b644-34f820e8111a.png" height="75%" width="75%" alt="AD DS"/>
  
<p/>

- Install Active Directory Domain Service
- Ensure you select Active Directory Service
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168498450-c9942468-6403-44eb-993c-73f566a718c7.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>

- After the installation process, click <em> promote this as domain controller </em>
- This allows other VMs to domin-join the domain controller
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168501928-11181ad1-fc56-4946-8993-272627459068.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>


# How to ensure VMs use our VM (VM with AD DS installed) as a DNS Server?
- To ensure that VMs to pick up the domain controller as the internal domain, IT admins must directory server becomes the DNS server for the VNet. 
- Place the private IP of the first VM (On-Premise AD) created as the Custom DNS Server and add it as a custom DNS server. 
- Remember we installed a DNS service on our VM.

- Take private IP of onpremAD VM (the domain controller) and use it as custom DNS server
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168506163-dba45c7f-77f9-4903-8e13-6bd0dbdc6cc1.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>

- Enter private IP of onpremAD VM into the DNS Server blade of the Virtual Network
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168506256-74335d95-cfa9-497d-8d14-7667e38a4208.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>



# Spin up a new VM and domain join onto the On-Premise Active Directory VM. Call it <em> webserver. </em>
- When we domain join the webserver VM onto the domain, IT admins can view the VM within the Active Directory
- Domain join webserver VM onto On-Premise AD VM

- RDP into webserver, navigate to local server, select workgroup> change> enter root-name 

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168501816-16265fcf-61a1-4f68-a89f-7286418de6e8.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>

- IT admins will be prompted to enter the domain controller's username and password
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168511931-4f271b1c-24d0-42a5-aec9-18104d0d746f.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>

- To verify if webserver is domain-joined, navigate to local server.
- Another method to check is to sign into webserver using admin credentials

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168513154-d24dbce3-f079-4e8d-ae05-18f67bd55e86.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>

# Create a new VM - adconnectserver1 to install Azure AD Connect onto the server. Call it <em> adconnectserver </em>
- AD Connect must be on a Windows 2019 Server
