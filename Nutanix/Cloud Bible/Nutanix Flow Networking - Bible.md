
### Core Use Cases
- Multi-tenant networking
- Network Isolation for Sec
- Overlapping IP addrs
- Self-Service Network Creation
- VM IP mobility
- Hybrid Cloud Connectivity

Note: Flow is managed via PC and is supported with AHV

### Implementation Constructs
- **VPCs**
	- Isolated network namespace with a virtual router instance to connect all of the subnets inside the VPC
	- Allows for IP overlap with other VPC or physical network
	- Can expand to include any cluster managed by same PC (Typically should only exist in a single cluster/Availability Zone) ![[Screenshot 2026-03-30 at 8.52.38 AM.png]]
- **Overlay Subnets**
	- VPC can have one or more subnets connected to the same virtual router
	- VPC leverages Geneve encapsulation to tunnel traffic between AHV hosts as needed
	- Subnets inside VPC dont need to be created or presented on the TOR switches for vms on different host to comm
	- When selecting a NIC for a VM, you can place it into an overlay subnet or vlan backed subnet, when an overlay is selected that is choosing the VPC
		![[Screenshot 2026-03-30 at 8.57.15 AM.png]]
	- NOTE: VM CAN ONLY EXIST IN **ONE** VPC
- **Routes**
	- Each VPC contains one Virtual Router with a few types of routes
		- External Networks
			- Should use default destination 0.0.0.0/0 network prefix for the whole VPC
			- You can choose an alternate network prefix for each external network in use
			- For completely isolated VPCs, you may set the default to none
		- Direct Connections
			- created for each subnet in VPC
			- Flow assigns first IP addr as default gateway for subnet (Determined by config and cannot be altered directly)
			- Traffic between two subnets on same host and same vpc will be routed locally 
		- Remote Connections
			- VPN connections and external networks can be set as the next hop destination for a network prefix 
	- EACH NETWORK PREFIX SHOULD BE UNIQUE IN VPC ROUTE TABLE
- **Policies**
	- Virtual router acts a control point for traffic into VPC and we can apply simple stateless polices here
	- Traffic from vms in the same subnet wont go trough policy
		![[Screenshot 2026-03-30 at 9.35.45 AM.png]]
	- Priority is evaluated from highest (1000) to lowest (10)
		- How to match traffic?
			- Source/Dest IPv4
				- Any
				- External to VPC
				- Custom IP Prefix
			- Protocol
				- Any
				- Protocol number
				- TCP
				- UDP
				- ICMP
			- Source/Dest Port # 
			- Actions
				- Permit
				- Deny
				- Reroute
					- Redirect traffic to another /32 IPv4 addr in another subnet
	- reroute policy is helpful to take action like routing all traffic to load balancer or firewall VM running inside another subnet in the VPC. Its added the value of only requiring a single network function VM (NFVM) for all traffic inside the VPC, rather than traditional service chain that requires and NFVM per AHV host
- **External Networks**
	- Created on PC and exist on only a single PE cluster
	- Network defined the VLAN, default gateway, IP addr pool, and the NAT type for all VPCs using it, one external network can be used by many VPCs
	- Max of two External network per VPC, and one NAT and one Routed External Network in same VPC
	- Types:
		- **NAT**
			- External network hides the IP addr of VMs in the VPC behind either a Floating IP or the VPC SNAT (Source NAT) addr
			- Each VPC has an SNAT ip addr selected randomly from the External Network IP pool, and traffic exiting the VPC is rewritten with this source addr
				![[Screenshot 2026-03-30 at 11.04.49 AM.png]]
			- Floating IP addr is selected from External Network IP pool and assigned to VMs in a VPC to allow ingress traffic
			- When a Floating IP assigned to a VM, the egress traffic is rewritten with the Floating IP instead of the VPC SNAT IP
		- **Routed (NoNAT)**
			- Allows IP addr space of Physical Network to be shared inside the VPC through routing
			- VPC router IP is selected randomly by External Network pool
				![[Screenshot 2026-03-30 at 11.10.12 AM.png]]
		- **Multiple External Networks**
			- Use cases:
				- Connectivity to internal and external resource through separate routes
					- Internal or Corporate through Routed, or NoNAT
					- External or Internet Service though NAT and Floating IPs
				- Providing Floating IPs for network gateway VMs inside the VPC. Network gateways inside a VPC require Floating IPs
					- VPN Network Gateway VM
					- VXLAN VTEP Network Gateway VM
					- BGP Network Gateway VM inside the VPC
			![[Screenshot 2026-03-30 at 11.28.46 AM.png]]
- **Network Gateway**
	- Connector between subnets, and can be of different types and locations
		- Subnet Types:
			- ESXi VLAN
			- AHV VLAN
			- VPC overlay (network gateway in a VPC require floating IPs)
			- Physical Network VLAN
		- Locations:
			- On-Pren VLANs
			- On-prem VPCs
			- Cloud VPCs
	- **Layer 3 VPN**
			![[Screenshot 2026-03-30 at 1.20.14 PM.png]]
		- When using 2 network gateways, each gateway VM is assigned an external floating ip addr from the NAT external network DHCP pool, and they must be able to comm over these addrs
		- when net gateway is inside a VLAN, the assigned IP addr must be reachable from the remote site
		- you can also connect net gateway to a remote phys firewall or VPN appliance or VM
		- local network gateway must still be able to comm with the remote phys or virt appliance
		- traffic is from each subnet is directed over the created VPN connection using IP routing inside the VPC.
		- Routes can be static or shared between network gateways using BGP
			![[Screenshot 2026-03-30 at 1.27.58 PM.png|589]]![[Screenshot 2026-03-30 at 1.34.43 PM.png]]
	- **Layer 2 Extended Subnets with VXLAN**
		- Layer 2 VXLAN VTEP![[Screenshot 2026-03-30 at 1.38.51 PM.png]]
		 ![[Pasted image 20260330134504.png]]
		- Layer 2 VXLAN VTEP over VPN
			![[Screenshot 2026-03-30 at 1.45.51 PM.png]]
	- **BGP**
		- automates the exchange of routing info between a VPC and an external BGP peer
		- use when VPC has a Routed or NoNAT external network along with an overlay subnet whose IP addr space has a req to comm with external network without using NAT, and comm needs dynamic addr advertisement
		- BGP gateway is not in the data path just part of the control plane
			![[Screenshot 2026-03-30 at 1.52.08 PM.png]]
		- Deployment:
			- can be deployed on an underlay VLAN-backed network or within the VPC in an overlay subnet
			- establishes peering session with upstream infra routers
			- peering session is used to advertise (up to 20 routable prefixes) the reachability of the configed routable VPC subnets to the BGP neighbor
			- next hop advertised from the VPC BGP gateway is the VPC router interface that is connected to the NoNat or Routed, external subnet
			- gateway supports one session per BGP peer and up to 10 unique BGP peers![[Screenshot 2026-03-30 at 2.36.24 PM.png]]
			- Deploying BGP gateway in a VLAN backed underlay network makes config easier as there are no reqs for NAT external network and floating IP assignment![[Screenshot 2026-03-30 at 2.42.43 PM.png]]