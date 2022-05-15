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

# Create a VM and Install AD DS on VM and ensure a Domain is in place. Call it <em> OnPremiseAD. </em>
- This VM will act as your on-premise Active Directory environment. 

# How to ensure VMs use our VM (VM with AD DS installed) as a DNS Server?
- Ensure our directory server becomes the DNS server for the VNet. 
- Place the private IP of the first VM (On-Premise AD) created as the Custom DNS Server and add it as a custom DNS server. 
- Remember we installed a DNS service on our VM.

# Spim up a new VM and domain join onto the On-Premise Active Directory VM. Call it <em> webserver. </em>
- When we domain join the webserver VM onto the domain, IT admins can view the VM within the Active Directory
- Domain join webserver VM onto On-Premise AD vm 

# Create a new VM to install Azure AD Connect onto the server. Call it <em> adconnectserver </em>
- AD Connect must be on a Windows 2019 Server
