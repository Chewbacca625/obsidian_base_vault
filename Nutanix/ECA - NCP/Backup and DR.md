- Backup Strategies:
	- per-vm: specific VMs backed up to a different site
	- selective, bi-directional replication: replication to both a primary and secondary site when required
	- synchronous datastore replication (Metro Avail): datastores spanned across two sites 
- Supported topologies
	- One-to-many: optimizes bandwidth
	- Many-to-one: hub-spoke architecture and operation efficiency
	- Many-to-many: most flexibility, and maximum amount of control to ensure app and service level continuity 
	- Two-way mirroring: active workloads run on each site

### Terms & Concepts
- Consistency Group: subset of entities in a protection domain, system captures snapshots for all entities within group for the protection domain in a crash consistent manner. Creates snapshot of all vms at the same time.
- Async Protection Domain: VM, VGs, or storage containers backed up locally on a cluster and optionally replicated to 1+ remote clusters
- Metro Availability Protection Domain: consists of a specific active storage container in a local cluster (active), this is linked to a standby container on a remote site
- Recovery Plan: Orchestrates the recovery of VMs at the recovery site 
- Snapshots: uses fewer resources compared to full backup because they capture the difference between the original state of your VM disk and its current state 

### Data Protection and DR Solutions
- Protection Domain Based DR is accessed and configed in PE and PC based DR is accessed and configed via PC. With replication schedules your can achieve RPOs of 0 seconds, 1-15 minutes, greater than an hour.
- Nutanix DR allows you to replicate from one on-prem cluster to another, accessed via PC using Protection policies.
- Finally, NC2 allows for cloud backups, replication between two NC2 clusters or on-prem cluster and NC2.![[Screenshot 2026-06-02 at 9.11.19 AM.png]]

### Replication Types Protection Domains PE
- Async:
	- Snapshot-based. It takes point-in-time, crash-consistent snapshots of the entire Protection Domain. Instead of sending the full data every time, it computes the differences (vblock diffs) between the current snapshot and the previous one, and sends only the changed data over the network to the remote site
	- RPO 1+ hours
- NearSync:
	- RPO of 1 to 15 minutes. Instead of waiting to take full vdisk snapshots, NearSync uses **Lightweight Snapshots (LWS)**. LWS captures data at the metadata level directly from the Oplog (the Nutanix write-ahead log on the SSD tier). It ships these actual write logs (episode data) to the remote site
- Metro/Synchronous:
	- Metro availability is a policy applied on a storage container, which effectively spans the storage container across two sites. This is accomplished by pairing a storage container on the local cluster with one on a remote site and then synchronously replicating data between the local (active) and remote (standby) storage containers. (DONT INCLUDE VGs)
	- ESXi / Hyper-V only (protection domains) - include storage only nodes which run AHV but not Hyper-V clusters with storage only nodes
	- witness vm: resides in separate network and monitors metro availability config health, automates failovers
	- Important considerations:
		- Acropolis Upgrades
		- Networking and Performance
		- Cluster Hardware Config & Sizing
		- VM Locality
- Cloud Connect
	- Allows you to configure a remote site to be AWS it stores extents in S3 buckets and Elastic Block Store is used to store metadata

### Data Protection Prism Central
- Nutanix DR works with a sets of Availability Zones, utilizes a policies and recovery plan approach. It uses categories to group entities and protect them. It offers network mapping, orchestration for DR recovery sequencing , inter-staging delays, and the ability to test application recovery without effecting production workloads.
- entities include:
	- guest VMs, VGs, and consistency groups
- Supported RPOs
	- Asynchronous (1 hour or greater RPO)
	- NearSync (1 to 15 minute RPO)
	- Synchronous (0 RPO)