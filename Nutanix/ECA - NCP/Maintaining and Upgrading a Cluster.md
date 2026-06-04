- Verify Cluster Health
	- Perform health check: make sure no errors with encryption certs or key manager not being are resolve or may result in data loss
	- Make sure node can tolerate one node failure 
		CLI way: **nutanix@cvm$ ncli cluster get-domain-fault-tolerance-status type=node**
- You can only place one node at a time in maintenance mode
- Non-migratable VMs (for example, pinned or RF1 VMs which have affinity towards a specific node) are powered-off while live migratable or high availability (HA) VMs are moved from the original host to other hosts in the cluster. After exiting maintenance mode, all non-migratable guest VMs are powered on again and live migrated VMs are automatically restored on their original host.
- You may encounter instances wherein an attempt is made to put a host into maintenance mode. However, VMs with GPU passthrough, CPU passthrough, PCI passthrough, and host affinity policies are not migrated to the other hosts in the cluster. As a result, these VMs may prevent attempts to enter maintenance mode.
- Host placed in maintenance mode -> migrate able vms moved / non migrate able manual shut down ->  CVM placed in maintenance mode (handling updates/unschedulable) -> upgrades take place -> CVM taken out of maintenence mode -> host taken out of maintenence mode -> resource migrated back and non-migrate able vms started up (maybe)

- Upgrading Memory (Depends on Hardware platform)
	- You can perform a non-disruptive memory upgrade on an AHV cluster, on one node at a time. At a high-level, this involves:
		1. Running a complete NCC health check.
		2. Identifying the node requiring memory upgrade.
		3. Shutting down the CVM and AHV host.
		4. Physically removing the node from the block and inserting the DIMMs.
- Replacing HDD and SSD
	- You can replace a drive in a node for the following failure scenarios:
	1. When a drive has failed completely.
	2. When the drive experiences recoverable errors and warnings that indicate potential failure. 
	
		At a high-level, this involves:
	
	3. Reviewing the drive failure indications.
	4. Identifying the proper drive slots.
	5. Running the NCC pre-checks.
	6. Verifying that the key management server is connected before replacing an encrypted drive.
	7. Preparing to replace the failed drive.
	8. Replacing the failed drive.
	9. Configuring the new drive.
	10. Running the drive replacement post checks.
- Replacing Boot/Metadata Drive
	- Each node in a block uses a single SSD drive as its boot drive, which may also store cluster metadata. If a node's boot drive fails, the CVM becomes unavailable until the malfunctioning drive is repaired. 

	Before replacing the drive, you need to log on to any CVM in your cluster and run the following command to collect the necessary information for replacing the failed SSD.  
	  
	
	**_nutanix@cvm$ ~/serviceability/bin/breakfix.py --boot --ip=cvm_ip_addr_**
	
	  
	
	At a high-level, the procedure to replace a boot/metadata drive involves:
	
	1. Making sure that the key management server is connected before replacing an encrypted drive.
	2. Preparing to replace the failed drive.
	3. Replacing the failed drive.
	4. Repairing the drive.
	5. Replacing the certificate signing request (CSR) for an encrypted drive.
	6. Adding the replacement drive to the cluster (if your cluster has more than one storage pool).
- Replacing NVMe Drives
	- Prism will notify you if an NVMe drive fails or experiences recoverable errors. In such scenarios, you need to replace the defective drive. At a high-level, this involves: 

		1. Clicking the **Remove** **Drive** button in the Prism prompt.
		2. Removing the locking screw from the drive carrier.
		3. Pressing the button on the front of the drive to release the lever.
		4. Grasping the lever and pulling the drive out from the node.
		5. Inserting the replacement drive in the empty slot and pushing it in until you feel resistance.
		6. Pushing the lever on the front of the drive down until it catches, and you hear it click in place.
		7. Tightening the locking screw on the drive carrier.
		8. Adding the drive.
		    1. If you are adding a clean drive, Prism will automatically add the drive.
		    2. If you are adding a drive that has data on it, you will need to go to Prism, select the drive in the **Hardware** **Diagram** and then select **Repartition** **and** **Add**.
- Replacing NICs
	- You can identify NIC failures in a node by checking for the following indications:

		- Performance of guest VMs decreases.
		- Guest VMs, the Nutanix web console, and nCLI are not accessible.
		- An error message occurs when attempting to migrate VMs.
- Recommended Update Order
	1. On Prism Central clusters, perform an LCM inventory.
	    1. Upgrade LCM framework. Do not upgrade any other software component except LCM.
	    2. Upgrade and run Nutanix Cluster Check (NCC).
	    3. Upgrade Prism Central.
	2. On Prism Element, perform an LCM inventory.
	    1. Upgrade LCM framework. Do not upgrade any other software component except LCM.
	    2. Upgrade and run NCC.
	    3. Upgrade Foundation.
	    4. Upgrade Cluster Maintenance Utilities.
	    5. Upgrade AOS.
	    6. Perform firmware updates (BIOS/BMC/Host boot drive or other critical firmware as recommended by LCM).
	    7.  If you are running AHV continue with the following steps. Otherwise skip to step h.
	        1. Upgrade AHV.
	        2. Perform LCM inventory again and upgrade any available firmware.
	    8. Perform these steps if you are running a hypervisor other than AHV. Otherwise skip to step i.
	        1. Upgrade hypervisor other than AHV.
	        2. Perform LCM inventory again and upgrade any available software or firmware.
	    9. Run an NCC check again.
	3. After performing core upgrades on Prism Central and Prism Element, upgrade Services software such as Prism Self Service, Files, Nutanix Kubernetes Engine, Objects, and other Nutanix software installed on Prism Central.