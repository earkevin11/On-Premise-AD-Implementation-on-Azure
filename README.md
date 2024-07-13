# On-Premise-AD-Implementation-on-Azure

Requirements:
- Azure VM based on Windows Server 2019
- Install Active Directory Domain Services role
- Installing AD role will automatically install DNS Service

# What is a domain controller?
- Domain controller is a server on the network that centrally manages access for users, PCs, and servers on the network. 
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
- Active Directory uses Domain Name Server (DNS) to find and resolve domain names into IP addresses.
- Active Directory cannot work without DNS.

# Use Case: Install AD connect on on-premises AD environment and sync users to Azure AD environment
- Since I do not have a on premise environment, an Azure VM will act as an on-premise AD environment.
- Three VMs will be needed
- One VM will act as a On-Premise local AD called <em> On-Premise AD </em>
- One VM will be the server where AD connect is installed. It will be called <em> adconnectserver </em>
- One VM will be <em> webserver </em> and we will domain join it to the local AD named On-Premise AD.
- FAQ: Is it ok to install AD Connect or Entra Connect Sync on the domain controller? It is supported however best practice is to separate the AD Connect Sync application and your domain controller. A good rule of thumb, a domain controller is a domain controller and should be nothing else. Typically, when you install a domain controller, you want to make sure there are no other services that interfere or compete with the compute, memory, networking, or disk resources. Recommended installation is always in a separate server regarding to isolate points of failure.

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168880072-64a1dee4-e2f0-41eb-ab5a-cec01693b6d9.png" height="105%" width="105%" alt=""/>
  
<p/>

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
- When you promote it as a Primary Domain Controller, you will need to create a new forest because you do not have a forest yet.
- You will notice that once you create your root domain name, after restart, in local server > domain > it will show as the root domain name you entered and will no longer show as "WORKGROUP"
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168501928-11181ad1-fc56-4946-8993-272627459068.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>


# How to ensure VMs use our VM (VM with AD DS installed) as a DNS Server?
- To ensure that VMs can pick up the domain controller as the internal domain, IT admins must make our on premise server to become the DNS server for the VNet. 
- Place the private IP of the first VM (On-Premise AD) created as the Custom DNS Server and add it as a custom DNS server. 
- Remember we installed a DNS server service on our on premise AD server VM.

- Take private IP of onpremAD VM (the domain controller) and use it as custom DNS server
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168506163-dba45c7f-77f9-4903-8e13-6bd0dbdc6cc1.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>

- Enter private IP of onpremAD VM into the DNS Server blade of the Virtual Network
<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168506256-74335d95-cfa9-497d-8d14-7667e38a4208.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>



# Spin up a new VM and domain join the web server onto the On-Premise Active Directory VM. 
# Call it <em> webserver </em>
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
- AD Connect must also be domain joined to the on prem AD server. And it be on a Windows 2019 Server

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168625972-2e2a6ee9-464e-417d-b5cb-28fbf8231b69.png" height="75%" width="75%" alt="AD DS "/>
  
<p/>


# Create two users in your Active Directory - onpremAD Virtual Machine
- Navigate to Tools > Active Directory Users and Computers > Users > New 
- Create two users to sync onto Azure AD environment

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168626871-25d78b4c-edb6-49da-afbf-9d50715bfb3e.png" height="190%" width="190%" alt="AD DS "/>
  
<p/>

# Verify that two domain users are created in the domain controller

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168628168-a126bc43-0092-4d76-9927-c1f711df3c8d.png" height="150%" width="150%" alt="AD DS "/>
  
<p/>

# Download AD Connect onto the adconnectserver VM
- Ensure that IE Enhanced Security Configuration is turned off so AD Connect can be installed
- Click download > Open folder and install > Use express settings 
- Admins must be Global Admin. If not, assign the Global Admin role.
- Then sign in with on prem administrator credentials
- When installing AD Connect, users should be automatically synced from on-premise AD over to Azure AD.

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168629415-100d7be5-e968-4cdf-bd88-5ca66bb0cf52.png" height="150%" width="150%" alt="AD DS "/>
  
<p/>

- Verify on Azure AD that the users created within the onpremad VM was synced

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168636174-f6e86bd5-24c9-4453-96ca-cac44a65b966.png" height="150%" width="150%" alt="AD DS "/>
  
<p/>

- domainUserA and domainUserB should be in the Azure AD directory
- Congrats! The users created from the on-premise AD environment have been synced onto the Azure AD environment
- Users created from on-premise environment can now log onto the Azure Portal using their credentials
- Note that whatever settings an IT admin applied for sign-ins does not apply since it is Pass-Through Authentication (On-Prem first)

<p align="center">
  
<img src="https://user-images.githubusercontent.com/104326475/168652337-47de586a-8ce1-48cf-8064-aab2f8c92bdb.png" height="150%" width="150%" alt="AD DS "/>
  
<p/>




