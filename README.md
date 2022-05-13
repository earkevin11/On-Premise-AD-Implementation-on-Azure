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

# What is a DNS Server?
- Domain Name System(DNS) is a server that is specifically used for matching website names (like google.com or microsoft.com) to their corresponding IP addresses.
- DNS Server contains a database of public IP addresses and their corresponding domain names.
- Think of a phone book where users can search for a person and then retrieve their phone number.

# Why is DNS server important to Active Directory?
- Active Directory uses uses DNS to find and resolve distinguished names into IP addresses.
- AD cannot work without DNS.


# Install AD DS on VM and Ensure a Domain is in place
- This VM will act as AD. 

# How to ensure VMs use our VM (VM with AD DS installed) as a DNS Server?
- Ensure our directory server becomes the DNS server for the VNet. -73
- Place the private IP of the first VM created as the Custom DNS Server and add it as a custom DNS server. Remember we installed a DNS service on our VM.

# Spim up a new VM and domain join onto the Directory
- When we domain join onto the domain, IT admins can view the VM within the Active Directory
