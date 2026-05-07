### Prereqs for Deploying a VM
- Network:
	- VM network interface is bound to virt network, and each virt network is bound to a single vlan
	- Managed (Nutanix IPAM), unmanaged (external IPAM)
- Image: 
	- used for OS and app install
- VirtIO Drivers (**WINDOWS**) - [link](https://portal.nutanix.com/page/downloads?product=ahv&bit=VirtIO)
	- drivers to enhance stability and performance of VMs
	- bundles eth adapter, RNG device, SCSI pass-through, SCSI controller, serial driver, etc.
	- When installing windows fresh use the VirtIO ISO, but when updating an existing driver use the MSI
- Categories: Apply entities to categories, then you can set polices on categories for governance
	- useful for management, and is recommended
- UEFI & BIOs
	- AHV can run on hardware that supports UEFI and Secure Boot of UEFI
		- VMs can be enabled to use Secure Boot
		- UEFI - faster boot times, supports larger disks, and provides more security (auth components to execute if boot loader has been tampered with)
	- Min version 1.1.6 of Nutanix VirtIO
- Sysprep
	- Used for windows installs, generalizes installs so it can be cloned for use across multiple types of hardware, removes windows Security IDs (SIDs) that are unique to each system and hardware-specific info
	- sysprep must be preinstalled in image to use 
- Cloud-Init
	- used to customize linux installs
	- cloud init must be preinstalled on image to use
- Agent VM: a specialized VM thats pinned to a specific physical host
### Prism Self-Service Roles
- Self Service Admin: can create projects, add users and groups to projects, config roles for each project member, publish vm templates and images, monitor project resource usage and adjust quotas, no admin access of PC, but has full access to all VMs running on a cluster
- Project user: consumes resources, can work with entities based on roles/perms, GUI only displays what user have access to.

### Self-Service Admin Tasks
- Config AD with a pool of self-service users
- Open self-service user role -> create authorization policy choose role, define scope, and assign users

### Nutanix Guest Tools
- NGT installer allows you to install NGT in a guest VM
- Nutanix Guest Agent, maintains comm between VCM and guest VMs
- Nutanix VirtIO package, vm mobility drivers that enable VM migration between AHV and ESXi, in-place HV conversion, and cross HV disaster recovery
- Nutanix VSS package, enable nutanix native in-guest volume snapshots service (VSS) agent to take app-consistent snapshots for all VMs that support VSS
- Python Package, NGA service is written in python and is a dedicated install

NGT Features:
- Self-Service file recovery: file-level recover using VM snapshots
- Nutanix VM mobility: Facilitates VM migration between ESXi and AHV, in place HV conversion, and cross HV DR
- VSS requester and hardware provider for Windows VM: Enables app consistent snapshots of AHV and ESXi
- App-consistent snapshot for Linux VMs: support app consistent snapshots for Linux running specific scripts on VM quiesce
- Static IP preservation support after failover for Nutanix DR: Preserves IP addr of guest VM with static IP for its failover DR to an IPAM network
- In-guest script exe support for Nutanix DR: automates various task exe upon recovery of the VMs
> Note: You can install NGT while a VM is running, but it will require a restart so for running workloads schedule or do in maintenance window

>**NUTANIX GUEST TOOLS VIA PE MUST BE MANUALLY ENABLED AND THEN INSTALLED BY LOGGING INTO THE VM, PC DOESN'T REQUIRE YOU TO LOGIN**

### Managing VMs
- PC: you can create templates, protect/unprotect, enable/disable efficiency measurement, anomaly detection, run playbooks, manage categories, manage ownership, apply storage policies, etc.
- You can also take actions on multiple VMs
- You must use nutanix move to migrate VMs from one cluster to another, but if you want to migrate VMs between node in a cluster this is possible without move

### Hot-Plugging CPU, Memory, and vNIC
- You can change the number of CPUs (vCPU/Socket), while a VM is turned on, but not # of cores per CPU
- You can also hot-swap memory, while VM is turned on
- Each action must be done one at a time (action, save, action,...)
- you can hot swap NIC it is dependent on successful ACPI request, if not it must be power cycled 

### Cloning VMs
- you can clone 250 VMs at a time
- you cannot override the secure boot setting while cloing a VM, unless the source VM already had secure boot enabled
- you cannot add or remove disks

### Enabling VM HA
- VM HA ensures that VMs can be migrated and restarted on another AHV host in an AHV cluster when a host becomes unavailable, it also respects affinity (if pinned to A and B in a cluster with ABCD, wont be started on CD) and anti-affinity rules
- Two Config options
	- Default: if enough resources exist VMs will be migrated to another host
	- Guarantee: if enough resources **DONT** exit then guarantee ensure that the VM will be spun up on another host (Reserves Space)
- Enabled in settings menu of PE - Data Resiliency - manage VM HA

### Manual vs. Automatic VM Migration
- Manual: moving one vm to another host within the same cluster or to new cluster(requires Nutanix Move) entirely (Non-Nutanix Storage - > Nutanix Storage, ESXi/Hyper-V -> nutanix cluster)
- VM Live Migration: VM moved from one node to another while VM is powered on (manual/automatic)
	- Live migration occurs automatically, it can be due to Acropolis Dynamic Scheduler (ADS) monitoring a cluster for hotspots and contentions and making adjustments
	- Micro Stunning (AOS 6.8) enables faster & more efficient migrations: replicates memory state of a VM from source VM host to destination VM host, and then cutting over. Due to VM memory changes are faster than VM memory can be replicated the VM is temporarily stunned to prevent memory updates and active state is moved to the destination host.
- Live migration limitations : [Link](https://portal.nutanix.com/page/documents/details?targetId=AHV-Admin-Guide-v11_0:mul-vm-live-migration-restrictions-c.html)
- ADS Automatic Migration:
	- ADS proactively monitor your cluster for any computer and storage I/O contention or hotspots over a period of time. If a problem is detected it migrates VMs from one node to another within a cluster
	- ADS runs every 15 minutes, if node is >85% utilization for 15 minutes, migration tasks are triggered, for storage it looks at the last 40 minutes of data and uses a smoothing alogrithem to use the most recent data, for CPU it looks at the average usage for the last 10 minutes (ADS doesn't monitor network or memory)
- Automatic Live Migration During Maintenance: if a node is put in maintenance mode, its marked as unscheduleable so no new VMs can be created on it, and ones running on it are evacuated to another host, and returned to original host