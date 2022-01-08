# Azure-Bastion
Description

Azure Bastion is a fully managed PaaS-Service, which allows to securely access an VM within a VNET. 

There are 2 methods to access the VM`s in the Azure Cloud:

            1.)	If the Azure VNET is connected with the local network via VPN, it`s possible to connect to the VM over the internal network via RDP. In this case, the              VM has     no public IP.

            2.)	Access the VM through a public IP address, which is assigned due the creation of the VM(standard). The NSG is used to activate the RDP/SSH port. Thus,              the VM     is accessible form the Internet, which for many servers is a high security risk.

Azure Bastion offers here a good solution to be secure and able to directly access the VMs over the Internet. The machines are still accessed via the classic remote protocols (SSH/RDP), but directly from the Azure portal. The portal is accessed via verified SSL, the connection to the VMs takes place over an own, secured Subnet via the internal, non-public IP addresses.


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


How to create Azure Bastion


![grafik](https://user-images.githubusercontent.com/97125784/148651474-a1cb7368-9dc0-4504-a126-82e9fbff823c.png)

Implementation

1.)	Creation of the special subnet in the corresponding VNET, it has to be named “AzureBastionSubnet” and be at least a CIDR /27 subnet.

2.)	Then create the Azure Bastion by using the Marketplace. During the configuration, in addition to the standard parameters(Subscription, Resource Group, Name, Region), only the corresponding VNET has to be selected, in which the previously configured “AzureBastionSubnet” is located.

3.)	The creation of the Azure Bastion NSG takes place in one of the next steps. The NSG must namely contain exactly the given bastion configuration, otherwise it cannot be bound to the subnet. Furthermore, there is no binding of a NAT-Gateway or an individual routing table to the “AzureBastionSubnet” allowed. After successful verification, an public IP address is further required to establish a direct SSL connection from an endpoint device(HTML5 Browser) to the VM in the VNET.

4.)	After the Azure Bastion PaaS environment has been provisioned, the NSG must be configured and connected to the subnet. 

5.)	When linking the NSG to the subnet, a check is carried out in the background. If the port and Protocol configurations not meet Microsoft's specifications, no link to the subnet can be established and the Azure Bastion Service is not active. 

Required inbound rules: 

    •	Port 443 must be allowed from the Internet (service tag Configuration)

    •	Incoming traffic from the Azure cloud (service tag) must be allowed

    •	Incoming traffic from the gateway manager (service tag) must be allowed 

Required outbound rules: 

    •	Outbound traffic for the remote protocols (SSH port 22 and RDP Port 3389) must be allowed

    •	Outgoing SSL traffic (443) to the Azure Cloud (service tag) must be allowed

