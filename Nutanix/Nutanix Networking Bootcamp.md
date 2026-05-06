- Customers dont do enough planning for the networking...
- IP required in foundation
	- CVM
	- Host
		- NOTE: CVM and HOST IPs must be in same subnet and broadcast domain
	- IPMI - one per host
		- doesnt need to be on same broadcast domain
		- but it can be
	- Gateway at least one for CVMs and Hosts
	- One Gateway for IPMI
14 ip for a 4 node cluster
- CVM x 4
- Hosts x 4
	- same broadcast domain
- 1 Gateway
- IPMI x 4
	- 1 gateway
- 2 x gateways
- 4 CVM and 4 Host
- 4 IPMI
- Multiply by 3 x ip addrs required for cluster (14x3= 42)
- Whites space - opportunities that are untapped
- Greenfield - New deployment
- Brownfield - Shitty upgrade
- Double subnet size for growth
- Discovert
	- Is this a new subnet for IPMI? (helps understand what IPs are available)
	- Why do you require a small subnets?
- Dont sell 1175s 3 nodes
- Keep network installation docs

Micro segmentation:
- Granular policies
- Virtual or overlay networks
- East-west traffic
- identity based/workload level
- software

Segmentation
- coarse policies
- physical network
- north-south
- address based/network level
- hardware

- Licensing is tied to node serial 
- IPMI is node specific dedicated (Base-T)
	- if it comes with based 10 AIOM (shared IPMI) - eth0 (data and IPMI access)
	- ASK about cables so that we can size correctly...!!!!!!!!!
	- Check with distribution, partners, etc. because nics
		- ONLY SUPPORTS if its on the board, no adapters
	- SFP+
	- We only support multimode, not single mode
	- What is AOC fiber? - Active optical cables draw power 
- Most nutanix nodes base-T is capped at 10Gbps
- IPMI mostly 100Mbps/1Gbps/10Gbps Base-T
	- for shared they use the first base-T interface (Cant be SFP)
- SFP+ capped at 10Gbps
	- Make sure that the SFP+ is compatable, any SFP interfaces need to also be compatable with the nic KB13531 SPF Not compatable with NIC, ATTENTION TO DETAIL!!!!
- Order matters when you take out disks and host from a block...
- Disk performance is directly related to network performance
- Active-Active Balance slb (MAC_Pinning) - DONT RECOMMEND
	- Multicast will not work b/c of ovs??? KB
- Active-Active LACP
	- LCAP - requires fallback (active backup should be enabled)
	- MUST BE IDENTICAL Interfaces
	- Wont disconnect, if a node fails but if you replace the nic it needs to be reenumerateed (there is a script)
- LC and MPO - cables are the most common fiber connection types nutanix
- all interfaces need to be the same, speed, driver, type, model (TO BE IN THE SAME BOND) - if they differ they cant be in the same bond
- Each bridge can only have one BOND
- Best practice is to seperate CVM and host traffic from workload traffic via vlans or bridges (physical)
- Setup
	- Plug in IPMI first
		- You can set it in trunk mode for added security, but typically not necessary
	- We use MLAG - for to connect switches enable LACP
	- Then We setup CVM host 
	- Then Workloads 
	- Br0 = host
	- Vs0 = cluster
  
  
  
  
  
https://portal.nutanix.com/page/documents/solutions/details?targetId=BP-2071-AHV-Networking:load-balancing-in-bond-interfaces.html

Review:
Discovery / Sales Specific Questions  
  
May I know the end user for this request?  
Do you have an approved Nutanix Deal Registration ID?  
Is this a new or existing Nutanix environment?  
Do you have an existing Nutanix Sizer Scenario or BOM for this request?  
Did you use a Discovery Reporting Tool (Nutanix Collector, RVTools, or Live Optics) to gather existing  
environment workload information?  
(If the customer is getting multiple clusters) Are you shipping all of the hardware to the same location?  
  
Hardware Specific Questions –  
  
(If Data collection tool is provided) Are you looking to count both powered on and off VMs or just powered on?  
Whats your vCPU workload requirement?  
How much RAM are you utilizing in VMs requirement?  
How much storage are you guys currently utilizing / looking for usable?  
Regarding your storage, are we looking at a all flash solution or a hybrid utilizing HDDs?  
Which type of network connectivity do you need on each server node? (10GbE, 25GbE, 40GbE, or 100GbE)  
For 10GbE, do you need SFP+ or Base-T connectivity?  
How many ports per host are you looking for?  
Does the customer have a GPU requirement?  
  
Software / Support Specific Questions -  
  
Which edition of NCI licensing do you need? (Starter, Pro, Ultimate)  
Nutanix Software Options Overview:  
h[ttps://www.nutanix.com/products/cloud-platform/software-options](https://www.nutanix.com/products/cloud-platform/software-options)  
  
How many years of software and hardware support do you need? (you can select anything between 1 – 7 Years)  
Are you looking for additional security/encryption or replication licenses?  
Are we looking for installation and startup services?