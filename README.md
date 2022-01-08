# Azure-Bastion
How to create a Azure Bastion

Azure Bastion is a fully managed PaaS-Service, which allows to securely access an VM within a VNET. 

There are 2 methods to access the VM`s in the Azure Cloud:

1.)	If the Azure VNET is connected with the local network via VPN, it`s possible to connect to the VM over the internal network via RDP. In this case, the VM has no public IP.

2.)	Access the VM through a public IP address, which is assigned due the creation of the VM(standard). The NSG is used to activate the RDP/SSH port. Thus, the VM is accessible form the Internet, which for many servers is a high security risk.

Azure Bastion offers here a good solution to be secure and be able to directly access the VM`s over the Internet. The machines are still accessed via the classic remote protocols (SSH/RDP), but directly from the Azure portal. The portal is accessed via verified SSL, the connection to the VM`s takes place over an own, secured Subnet via the internal, non-public IP addresses.


Azure Bastion – Functions:

•	Login to the Azure portal via SSL and MFA (recommended) 
•	The machines are accessed directly in the browser via SSL / TLS encryption and internal IP addresses
•	No port and protocol activation on the VMs necessary
•	No locally installed RDP or SSH client on the end device for Management of the VM`s required - the management of the VMs can be done by almost any modern end device
•	No need for jump hosts for remote support (partners)
•	Central monitoring of access and control via Azure RBAC


Requirements for Azure Bastion:

•	Creation of a special subnet (AzureBastionSubnet)
•	Each VNET has its own Azure bastion subnet and thus requires its own instance of the PaaS service
•	Configuration of a separate, completely secured NSG on the Azure Bastion subnet 
•	Securing access to the Azure portal via RBAC and MFA (recommended) 
