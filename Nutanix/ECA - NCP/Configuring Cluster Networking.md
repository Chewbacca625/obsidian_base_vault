### Networking Terms
- Open vSwitch (OVS): open source switch software for multi server virtualization. Each node contains OVS instance, manages as single logical switch via Prism. Behaves like layer 2 switch (MAC Address Table). HV connects via virt ports and it allows for features like VLAN tagging, Link Aggregation Control Protocol (LACP), port mirroring, and Quality of Service (QoS).
- Virtual Switch (VS): defines a collection of AHV nodes and the uplink ports on each node. Its an aggregation of the same OVS bridge on all the compute nodes in a cluster.
	- EX: vs0 is default virt switch and it aggregates br0 and br0-up uplinks from all the nodes
- Bridges: act as virt switches to manage phys net and virt net interfaces. Default AHV config is br0 and native linux bridge virbr0. virbr0 carries mgmt traffic and storage comms between CVM and AHV hosts. All other storage, host, and vm network traffic flows through br0.
	- NOTE: virbr0 handles internal network traffic on a host NO OUTSIDE COMMS. Its only comms between AHV host and local CVM. All local storage I/0 and mgmt traffic between the hv and the cvm pass through the native linux bridge.
- Ports: logical constructs created in a bridge to connect to virt switch. 
	- Types:
		- Internal Port: Used for host mgmt and typically has the default bridge name (br0)
		- Tap Port: connects vNICs to bridge
		- VXLAN Ports: used to provide IPAM functionality to AHV
		- Bonded ports: NIC teaming for phys interfaces of the host
- Bonds: aggregate phys interfaces on AHV host for fault tolerance and load balancing. Default, system builds bond br0-up containing all phys nics in bridge br0.
	- Load Balancing modes:
		- Active-Backup(Default): recommended, one interface is randomly selected to carry traffic and other interfaces are used as backups. Easily allows for multiple upstream switch connections without additional config.
		- Active-Active with MAC Pinning/Balance-SLB: takes advantage of all links in a bond and uses measured traffic load to rebalance VM traffic from highly to less used interfaces. When config bond rebalance interval expires, measured load for each interface and load for each source MAC hash to spread traffic evenly among links in the bond.
		- LACP with Balance-TCP: uses full bandwidth provided by multiple links to upsteam switches, from a single VM, requires dynamic negotiated link aggregation and load balancing using balance-tcp. Dynamic link aggregation is preferred over static for failure detection and recovery.
	- Note: LACP (Link Aggregation Control Protocol) can be activated for a bond to negotiate link aggregation with phys switch.
		![[Screenshot 2026-03-10 at 8.29.23 AM.png]]
- Bridge Chaining: 
	![[Pasted image 20260310100526.png|373]]
	> In the diagram above, brN.local connects to vms and then br.mx (multiplexer | FUNNEL: collects traffic from all local bridges and funnels it into the security chain), br.microseg (Holds flow rules for Network Flow and traffic is allowed or denied based on microseg policy), br.nf (third party services insertion (virt firewall) traffic is diverted here to be inspected), br.dmx (sorts traffic after its been inspected demultiplexer), brN exits

	- Allows multiple OVS bridges to connect in a straight line and is the backend of micro segmentation. Each bridge performs a specific set of functions. 
- Virt Local Area Network (VLAN): you can easily manage vNICs networks for user VMs using the Prism GUI, aCLI, Rest without any additional host config. Each vNet in AHV maps to a single VLAN and bridge. Each vNet in AHV maps to a single VLAN and bridge. You must create each VLAN and vNet in AHV and TOR switch, and integration between AHV and phys switch can automate provisioning.
	![[Screenshot 2026-03-10 at 10.54.12 AM.png]]
- IP Address Mgmt (IPAM): enable AHV to assign IP addresses to VMs using DHCP, vlan can be configed with subnet, associated domain settings, and group of IP address pools for assignment. AOS used VXLAN (virtual networking tech that wraps layer 2 data frames in layer 3 UDP packets so it can travel across layer 3 infra) and openflow rules to intercept DHCP requests from user VMs. Managed networks are one in which IPAM has been enabled.

	> Note: If IPAM is not enabled, its assumed that mgmt for this vlan is handled outside the cluster. Once enabled it can't be disabled

	![[Pasted image 20260310105941.png]]
- Availability Zones: distinct locations within a region engineered to be isolated, provide inexpensive, and low-latency network connectivity to protect agains failure of other AZ's
- ESXi or Hyper-V Equivalent Net Terms:
	![[Pasted image 20260310110130.png]]
- Max Transition Unit (MTU)
### Understanding Networking
- OVS run within AHV host apart of the linux kernel, the CVM is the control plane to manage OVS (OVS runs on each node)
	- All OVS instances combine to form a single logical switch
	![[Pasted image 20260310121342.png|373]]

### Configuring and Managing Networking in PC
- VPC: independent and isolated IP address space that functions as a logically isolated vNet. VPC is made up of 1+ subnets connected via logical or vir router.
- Floating IP: is an IP from an external subnet pool that is assigned to a VM in a VPC to provide external connectivity. It functions as a 1:1 static NAT mapping between private and public IP address.

### Creating & Updating vSwitches
- vs0 is the default:
	- Properties:
		- Cant delete
		- default bridge br0 on all nodes in cluster map to vs0. vs0 is not empty and had at least 1 uplink
- General config: name, description, and MTU value
- Uplink config: bond type, host, uplink ports, uplink speeds, and host ports

### Config & Managing Networks in PE
- To track and record net stats for a cluster, the cluster needs info about the first hop network switch ports being used. 
	- Requirements:
		- RFC 2674 compliant switch
- Discovery requires stats from the Q-BRIDGE-MIB on the switch and then identifies the MAC that corresponds to the host, discovery is best effort SNMP settings need to be configured for prism to config switch info
- To enable active-active balance-tcp you need to enable Link Aggregation Group (LAG) and LACP on the TOR Switch
	- uplinks might appear as single L2 link so a vm mac addr might appear on multiple links and use the bandwith of all uplinks
- Network Visualizer: helps visualize the phys and logical network topologies, including device type, network config on devices, and topology
	- Prereq's:
		- Config net switch info on cluster
		- Enable LLDP (Layer link discovery protocol (L2): advertises their identity and capabilities and neighbors on LAN) or CDP (cisco version) on first hop switches, if LLDP and CDP are unavailable SNMP will be used on a best-effort basis
		- Optional: Config SNMP v3 or SNMP v2c if you want to display switch interface stats
		  
> 	Network visualizer depend on DNS system to map switch hostnames and IP addresses based on LLDP


### Extending Subnets
- a subnet can be extended between on-prem and remote sites (AZ's) to support seamless migration of apps between sites.
- To extend subnet to support his functionality it must be configed in PC 
- L2 network extensions all you to migrate a set of apps to the remote AZ while retaining their network bindings such as IP, MAC, and default gateway
- Since subnet extension allows vms to comm over the same broadcast domain, it elims
	- re-arch network
	- downtime
- You can extend L2 subnet using either:
	- VPN between two nutanix AZs
	- VTEP between subnets across two Nutanix AZs as well as between a Nutanix AZ and one or more non-nutanix datacenters
- Supported config:
	- IPAM Type: managed/unmanaged
	- Subnet type: on prem vlan subnets and VPC subnets
	- Traffic Type: IPv4 unicast traffic and ARP
	- On-Prem HV: AHV or ESXi
- Prereq's:
	- Ensure PC version support L2 Net Extension
	- Ensure that you pair PC at the local AZ with PC at remote AZ to create extension
### Segmenting Networks
- Benefits:
	- Enhances Sec
	- Resilience 
	- Cluster Performance
- Isolating a subnet of traffic to its own network
	![[Screenshot 2026-03-12 at 9.58.12 AM.png]]
- Types of Traffic:
	- Backplane: intra-cluster traffic that is necessary for the cluster to function.
		- CVM traffic to hosts for rep, host mgmt, HA, VM live migration.
	- Management: admin traffic, or traffic associated with Prism and SSH, remote logging, SNMP, etc. Any traffic thats not backplane traffic
		- includes communications user VMs and CVMs 
		- mgmt plane can be segmented to isolate per service, ex external iSCSI traffic
- Unsegmented Net (Default): 
	- CVM has two vNICs. eth0 external switch, eth1 internal network and enables CVM comms with HV
	- CVM traffic whether backplane or mgmt traffic it uses interface eth0 (on default VLAN on default switch)
- Segmented Network:
	- mgmt traffic uses CVM interface eth0 and additional services can be isolated to diffrent VLANs or vSwitches. Backplane traffic use eth2, and you can define your own VLAN when segmenting
	- to isolate specific traffic you will need additonal vNICs on the CVM, no new vmkernel adapter or internal interfaces are req
- Types of Traffic Isolation
	- Isolating backplane traffic using VLANs (logical segmentation)
	- Isolating backplane traffic physically (physical segmentation)
	- Isolating service-specific traffic
	- Isolating Stargate-to-Stargate traffic over Remote Direct Memory Access (RDMA)