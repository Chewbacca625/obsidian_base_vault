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
	- What is AOC fiber?
- Most nutanix nodes base-T is capped at 10Gbps
- IPMI mostly 10Gbps Base-T
- SFP+ capped at 10G