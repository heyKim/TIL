# Network Interface
* What is a Network Interface
* Create a Network Interface
* View network interface setting
* Change DNS servers
* Enable or disable IP forwarding

## What is a Network Interface?
* 네트워크 인터페이스는 Azure VM이 Internet, Azure, on-premise resource와 communicate 할 수 있게 해준다.
* Azure Portal 에서 VM을 생성하면 default로 Network interface를 생성한다.
* Azure는 Network Interface가 VM에 attached되고 VM이 처음으로 start될 때 MAC address를 할당한다.

## Create a network Interface

|Setting|Required?|Details|
|---|---|---|
|Name|Yes| ex)  nic-<##>-< vm name >-< subscription >-<###> [Naming conventions](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming). The name cannot be changed after the network interface is created.|
|Virtual network|Yes| 생성 후 변경불가. network interface와 연결할 VM은 같은 location, subscription 안에 존재해야함.|
|Subnet|Yes| - |
|Private IP address assignment|Yes| In this setting, you're choosing the assignment method for the IPv4 address. Choose from the following assignment methods: **Dynamic:** When selecting this option, Azure automatically assigns the next available address from the address space of the subnet you selected. **Static:** When selecting this option, you must manually assign an available IP address from within the address space of the subnet you selected. Static and dynamic addresses do not change until you change them or the network interface is deleted. You can change the assignment method after the network interface is created. The Azure DHCP server assigns this address to the network interface within the operating system of the virtual machine.|
|Network security group|No| Leave set to **None**, select an existing [network security group](security-overview.md), or [create a network security group](tutorial-filter-network-traffic.md). Network security groups enable you to filter network traffic in and out of a network interface. You can apply zero or one network security group to a network interface. Zero or one network security group can also be applied to the subnet the network interface is assigned to. When a network security group is applied to a network interface and the subnet the network interface is assigned to, sometimes unexpected results occur. To troubleshoot network security groups applied to network interfaces and subnets, see [Troubleshoot network security groups](diagnose-network-traffic-filter-problem.md).|
|Subscription|Yes|Select one of your Azure [subscriptions](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). The virtual machine you attach a network interface to and the virtual network you connect it to must exist in the same subscription.|
|Private IP address (IPv6)|No| If you select this checkbox, an IPv6 address is assigned to the network interface, in addition to the IPv4 address assigned to the network interface. See the IPv6 section of this article for important information about use of IPv6 with network interfaces. You cannot select an assignment method for the IPv6 address. If you choose to assign an IPv6 address, it is assigned with the dynamic method.
|IPv6 name (only appears when the **Private IP address (IPv6)** checkbox is checked) |Yes, if the **Private IP address (IPv6)** checkbox is checked.| This name is assigned to a secondary IP configuration for the network interface. To learn more about IP configurations, see [View network interface settings](#view-network-interface-settings).|
|Resource group|Yes|Select an existing [resource group](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) or create one. A network interface can exist in the same, or different resource group, than the virtual machine you attach it to, or the virtual network you connect it to.|
|Location|Yes|The virtual machine you attach a network interface to and the virtual network you connect it to must exist in the same [location](https://azure.microsoft.com/regions), also referred to as a region.|


### 참고 사이트
* https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface

**(참고)NIC**
* PC나 서버 등의 컴퓨터를 네트워크에 연결시키기 위한 장치
* 랜카드, 네트워크 어댑터, 네트워크 인터페이스카드(NIC), 이더넷 카드라고도 함

