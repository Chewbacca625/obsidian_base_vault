-Upgrading manually you need to update firmware last
### Benefits of HCI
- Agile operations
	- HCI installer is packaged as a full stack installer allowing you deploy nodes quickly at multiple geo locations
	- You can scale linearly and predictably
	- No downtime upgrades because the CVM updates are isolated from workloads
	- Single pane of glass for management
- Sustained Innovation
	- Hybrid cloud can be implemented with no need to compromise on benefits of on prem and cloud
- Optimized Economics
	- HCI can scale dynamically with business need, further sub based licensing allowf for opex budget for datacenter infra

### Nutanix Cloud Platform
- NCI (Nu Cloud Infra): complete software solution including virt compute, stor, and net, for vm and containers, with built in DR, self-healing, resilience, performance, and sec
	- AOS
	- AHV
	- Nutanix Kub Engine
	- Nutanix DR
	- Flow Network Security
	- Flow Virt Networking
	- NC2
- NCM: Delivers intelligent ops, monitoring, insights, and automated remediation, cost governance, security operations for workloads and data across clouds
	- Intelligent Ops
	- Self-Service
	- Cost Governance
	- Nutanix Security Central
- NUS: distributed software defined storage solution that offers:
	- File storage
	- Volumes Block Storage
	- Object Storage
	- Mine Integrated Backup 
	- Intelligent analytics
	- Single management plane
- NDB: Nutanix database management across hybrid cloud offering powerful automation, scaling, patching, protection, and cloning of DB instances
- EUC: Nutanix desktop as a service solution built on NCI
### Nutanix Core Components
- AOS Storage: scalable, resilient, high performance, distributed storage solution. Eliminates the need for SAN and NAS solutions.
	- High performance storage. distributed data and local access to performance storage media like NVME and other high bandwidth media
	- Resilient and secure storage: protects against bit rot, hardware failure to physical theft and total site failure
	- Flexible and scalable (public or private cloud)
- AHV: offered as part of the NCP with no additional cost. Meets strongest security reqs, offers intelligent vm placement, live migration, hypervisor conversion, and cross cluster HA.
	- Ease of management
	- Native Security: security by default, offers network micro seg, builtin audit and remediation
	- Low ops cost
- Prism: Single management UI for monitoring and remediation end-to-end
	- RBAC 
	- automate workflow with REST API

### Understanding Nutanix Cluster
- Node: x86 server with compute and storage resources max # of nodes defined by HV
	- **Three Types:**
		- HCI node: cpu, mem, and storage
		- Storage-Only Node: Bare min CPU, but significant amount of storage
		- Compute Only Node: Bare Min Storage, but significant amount of CPU and Mem
	- NOTE: HCI nodes can be AHV, ESXi, or Hyper-V. **HOWEVER**, SO are AHV only, and CO nodes are only supported by AHV and ESXi
- Block: is a chassis that holds 1-4 nodes and contains power, cooling, and the backplane for the nodes
- Cluster: logical grouping of physical and logical components. Cluster can contain 1 or more nodes and can be house in one or more blocks. Its possible for nodes in a single block to belong to different clusters

### Nutanix Cluster Components
- **Acropolis:** Runs on every CVM with an elected leader. Follower is responsible for stat collection and publishing, and provides VNC proxy capabilities. Leader is responsible for stat collection and publishing, task scheduling and exe, vm placement and scheduling, network controller, and VNC proxy. 
- **Genesis:** Runs on each nodes and is responsible for service interactions (start/stop/etc.) and initial config. Runs independent of cluster and doesn't require cluster to be configed/running, but Zookeeper needs to be up an running for Genesis to work.
- **Zookeeper:** Stores info about cluster components (hardware/software), including ip, capacities, data rep rules, in the cluster config. Its active on 3 or 5 nodes, depending on redundancy factor applied to cluster. Uses multiple nodes to prevent stale data from being returned to other components. Odd number for breaking ties. One node is elected to be the leader by Zookeeper, it will receives all request for info and confers with follower nodes. Zookeeper has no dependencies.
- **Zeus:** Interface to access info stored in Zookeeper is used by all other components to access cluster config. 
- **Medusa:** Keeps track of stored data for other systems (hv or vms) and where replicas are stored. Its the abstraction layer that sits in front of the db that holds metadata. The db is distributed in a ring topology in the cluster for resiliency and is using a modified from of apache cassandra. 
- Cassandra: Stores all metadata about the guest vm data in a storage container. Runds on all nodes of the cluster, and communicate using the gossip protocol ever second. Depends on zeus to gather info about the cluster config.
- Stargate: Component for receiving and processing data to storage presented to other systems like hv. All r/w requests are sent across a vSwitch to the stargate process running on that node. It depends on medusa to gather metadata and zeus to gather cluster config data. Stargate the the main point of contact from the prospective of the HV
- Curator: Lead node periodically scans metadata and identifies cleanup and optimization that stargate should perform. Shares analyzed metadata across other curator nodes. Depends on Zeus to learn which nodes are available, and medusa to gather metadata, based on the analysis commands are sent to stargate.

### Nutanix Prism
- Central access for admins to config, monitor, and manage virtual env's by combining them into a single interface
- PE: Only manages the cluster that its part of, each cluster has its own instance of PE
- PC: allows you to manage diff clusters on one screen, can be in same location or geo distributed, provides an org view, ability to enable kub, self-service, etc.
- PE management:
	- Cluster mgmt: ability to monitor entities in cluster like vms, stor containers, hardware comps, etc.
	- Stor mgmt: Helps you monitor storage usage across cluster, ability to create stor containers and vg's, config threshold warnings, reserve storage cap
	- Net mgmt: allows you to track and record net stats for a cluster. On AHV, a network visualizer is provided that displayed a consolidated GUI of network formed by the vms and hosts in cluster
- Port to access cluster ip:9440
- Pulse: Provides diag system data to Nutanix customer support, allowing proactive, context-aware support and is collected in the background with little to no affect on system perfom. It is recommended to enable pulse, but if it violates sec policies then you can disable it with the help of nutanix support.
		- Shares:
			- Sys alerts
			- Current nutanix software ver
			- Nutanix Processes and CVM info
			- HV info, type & version
	![[Screenshot 2026-03-09 at 11.21.20 AM.png]]
- Cluster Details Window:
	- Cluster UUID
	- Cluster ID
	- Cluster Name
	- FQDN
	- Virt IP
	- Virt IPV6
	- ISCSI data services IP
	- checkbox for the retention of deleted vms
	- Cluster encryption state
- Prism Central: is an app that needs to be spun up as a vm before it can be used. You must register you clusters to the PC instance.
	- Additional feature over PE
		- Cost Governance
		- Resource planning: capacity runaway tab allows you to view current resource runway
		- Task automation
		- Self-Service Marketplace
- Note: Virt IP and ISCSI data service IP must be configed in PE
	![[Screenshot 2026-03-09 at 12.05.18 PM.png]]
- PC vs PE Capabilities:
	![[Screenshot 2026-03-09 at 12.09.07 PM.png]]
	![[Screenshot 2026-03-09 at 12.09.16 PM.png]]
### Performing Initial Cluster Setup
- Config Data Services IP: used to provide ISCSI access to your cluster storage. Used by Nutanix Volumes to present storage to guest vms directly via ISCSI. 
- Config Name Servers: Host a network service for queries against a directory service 
- Config NTP Servers: Network Time Protocol for clock sync between computers. Hosts and CVMS must be configed to synch system clocks with NTP servers.
	- Accurate time sync used for Nutanix DR and third-party backups solutions
	- Default is to use UTC
- Config SMTP: PC and PE use SMTP to send alert emails to exchange emails with Nutanix Support
- Config Auth: PC Supports SAML, Local User Auth, AD Auth
- Config UI Settings: Login screen animations and timeout settings
- Config Welcome Banner: can contain custom message and graphics