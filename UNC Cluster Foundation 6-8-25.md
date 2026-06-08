- DR Custer refoundation only vms running are the CVMs
- OEM - Nutanix hardware NX nodes
- First thing stop all replication jobs
- Unregister vsphere cluster https://portal.nutanix.com/page/documents/details?targetId=vSphere-Admin6-AOS-v7_3:wc-vcenter-server-unregister-t.html
- Lisencing get lisence details and download - backup file in PC - admin - lisencing - select manually manage - csf file - download - you have to manually claim the lisences back in portal before foundationing (you must unlisence/reclaim in the portal) - apply new lisence so it removes lisences in pc
-  need to disconnect cluster from PC
```
ncli remove cluster state

ncli multicluster remove-from-multicluster external-ip-address-or-svm-ips=<Prism_Central_IP> username=<PC_Admin_Username> password=<PC_Admin_Password> force=true

ncli multicluster get-cluster-state

cluter status | grep -v UP - checks all the cluster CVMs and services ect

cluster stop - stops all the CVM services



```
- setup foundation appliance
	- put it on same subnet as cluster
	- 