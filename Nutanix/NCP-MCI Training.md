### Managing Images
- When you upload an image you dump an image in a directory structure and we can categorize images and place images where we desire
- OV is a single-file distro that allows virual appliances to be exchanged across product and platforms
- You can create a VM from an OVA file and share with other users
- PC has a separate OVA dashboard 
- When an image is uploaded to PE so it needs to be pointed to in PC
- you can send quicklinks for virtio drivers, move, or PC bits, etc.

### Creating VMs
- Prereq:
	- images
	- networks
	- Virtio
	- Categories
- naming convention is important
- create one or more vms
- choose the cluster to create the VMs on
- Specify VM CPU and memory details
- specify advanced settings
- enable memory overcommit (not advised) - **LOOK INTO MORE**
- you need an iso for network, disk, etc. require virtio

### Managing VMs
- Acropolis Dynamic Scheduler
	- Runs every 15 min and analyzes resource usage
	- Monitors compute and storage utilization, and memory over commit if overcommit is enabled
	- eliminate hotspots within a cluster by migrating vms
	- 10Gb network interface most config matters around isolating CVM and workload traffic
	- **LOOK INTO** self service restore

### Monitoring Cluster Performance
- 


Image repo 10.42.194.11
