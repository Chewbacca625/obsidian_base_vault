- Verify Cluster Health
	- Perform health check: make sure no errors with encryption certs or key manager not being are resolve or may result in data loss
	- Make sure node can tolerate one node failure 
		CLI way: **nutanix@cvm$ ncli cluster get-domain-fault-tolerance-status type=node**
- You can only place one node at a time in maintenance mode
- Non-migratable VMs (for example, pinned or RF1 VMs which have affinity towards a specific node) are powered-off while live migratable or high availability (HA) VMs are moved from the original host to other hosts in the cluster. After exiting maintenance mode, all non-migratable guest VMs are powered on again and live migrated VMs are automatically restored on their original host.
- You may encounter instances wherein an attempt is made to put a host into maintenance mode. However, VMs with GPU passthrough, CPU passthrough, PCI passthrough, and host affinity policies are not migrated to the other hosts in the cluster. As a result, these VMs may prevent attempts to enter maintenance mode.
- Host placed in maintenance mode -> migrate able vms moved / non migrate able manual shut down ->  CVM placed in maintenance mode (handling updates/unschedulable) -> upgrades take place -> CVM taken out of maintenence mode -> host taken out of maintenence mode -> resource migrated back and non-migrate able vms started up (maybe)
