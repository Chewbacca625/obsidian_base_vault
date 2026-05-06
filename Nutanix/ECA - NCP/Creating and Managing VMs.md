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
- Python Package, NGA service is written in python 
