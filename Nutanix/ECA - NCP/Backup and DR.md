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