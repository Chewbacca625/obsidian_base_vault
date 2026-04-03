- Understanding Licensing
	- License manager: independent (Licensing as a Service) - integrates with support & insights portal page with agent software in PE and PC clusters
	- Can be upgraded with LCM
	- Ways to License:
		- Seamless: applies and manages licensing via PC web without the need to download and upload files | not available for dark sites
		- 3-Step Licensing: Mostly Manual, applying licenses to cluster via prism web console licensing page, requires internet connection
			- How to:
				1. Download cluster summary file
				2. Upload to Support & Insights Portal
				3. Download license summary file
				4. Upload license summary file to cluster
		- Dark site licensing : typically done with license key
	- To find Licenses view https://portal.nutanix.com
- Life Cycle Manager: framework & set of modules for inventory & updates (Hardware(Bios/Firmware/etc.) & Software(AOS/Prism/etc.))
	- Intelligently prioritizes updates
	- Automates entire upgrade process across all cluster hosts
	- LCM modules are independent from AOS
	- Supports firmware updates for:
		- Dell XC/Core, Lenovo HX/Ready, HPE DX, Fujitsu XF, Intel DCB, HPE DL (G10), Inspur In Merge, Cisco UCS M6/M7
	- LCM updates are not reversible
		![[Screenshot 2026-03-03 at 9.58.44 AM.png]]
	- LCM updates one node at a time, updates are batched to minimize downtime (bios updated first , then NC Platform, ect.)
	- Dark site: 
		-  PC: 
			1. Run LCM Inventory
			2. Updates LCM
			3. Run NCC
			4. Update Prism
		- PE:
			1. Run LCM Inventory
			2. Run NCC
		- Remaining Software:
			1. Run LCM Inventory
			2. Upgrade Foundation
			3. Upgrade cluster maintenance utils 
			4. Upgrade AOS
			5. Upgrade Firmware
			6. Upgrade AHV
			7. Run LCM
			8. Repeat steps if non AHV hypervisor
			9. Run NCC
			10. etc.
	- You can run NCC check from PE UI not PC
	- You can upgrade firmware in PE but not PC
- Performing Inventory:
	- You can view currently installed version of software and last update
	- Lists software and firmware needed to be updated
	- Available updates for the cluster
	- Can be done via internet or dark site
	- Recommendations:
		- Schedule Maintenance
		- Apply updates to non prod first
		- Update via prism using LCM
		- If you choose to manually update specific cluster comps, perform using nutanix update steps
		- Dark sites you can download binaries on to secure comps with secure portable media
		- You can temp disable email alert services or with nCLI cmd, once update is complete you can re-enable. Further, perform any emergency firmware updates as recommended by LCM
		- Wait for upgrades complete, before starting another
	- Node Maintenance Mode:
		- Typically done when network config changes, manual firmware upgrades/replacements, CVM maintenance, etc. that require node to not be operational
		- PE -> hardware -> table -> host -> enter maintenance mode -> power off VMs that cant migrate -> exit maintenance mode 
		- What happens when enabled?
			- AHV host inits maintenance mode
			- migrate-able vms (vm-host affinity | RF1) are powered off
			- Live migratable or HA vms are moved
			- AHV completes maintenance mode
			- CVM placed in maintenance mode
			- CVM shut down
			- Host marked unusable
		- What happens when disabled?
			- CVM powered on
			- CVM taken out of maintenance mode
			- Host taken out of maintenance mode
			- RF1 vms powered on and VMs migrate back to host locality
		- Dark site two ways to perform inventory:
			- web server:
				- Download latest LCM framework
				- transfer tar file to local web server 
				- extract contents into release dir
				- for each cluster access PE or PC perform inventory
			- directly upload:
				- Download latest LCM framework
				- transfer tar file to local computer
				- Access direct uploads PE/PC
				- Upload Bundle
		