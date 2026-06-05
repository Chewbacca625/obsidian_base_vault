Nutanix recommends that you upgrade to the latest applicable LCM version before running LCM operations on your cluster to avoid issues. You might encounter the following issues with respect to LCM. 

- Download failures - The LCM framework utilizes a connection with a pre-configured URL to identify the currently available updates, and perform its operations. A download failure occurs if the framework encounters errors while downloading contents from the URL. 
- Pre-check failures – A pre-check failure can occur during LCM’s inventory operation. You can view the reason for the failure in the LCM UI. 
- LCM framework issues – You can troubleshoot issues with the LCM framework by examining ERROR messages in the LCM logs. For instance, you may encounter messages such as LcmRecoverableError, LcmOpsError, LcmLeadershipChange, and others during LCM operations.
- XC Platform issues - LCM uses the PowerTools suite for firmware updates except for SATADOMs. The SATADOM module is shared between NX and XC clusters. Some of the issues during updates include failed iDRAC jobs and connectivity problems with Dell Update Manager (PTAgent). 
- Dark site firmware not reflecting correctly - After an LCM inventory operation is completed, the latest available firmware for nodes in the dark site cluster might not be reflected correctly. This could be because the dark site web server is incorrectly configured or inaccessible, or it could be caused by a violation of Foundation version dependency.
- Miscellaneous issues – You can encounter LCM issues in scenarios where modules require the node to boot into Foundation’s boot_into_phoenix and reboot_to_cvm APIs for firmware updates or when you upgrade LACP/LAG to the latest Foundation version. LCM updates can also fail due to bad disks, tokens, or filesystem errors on the /home partition of CVMs.
Managing Resource
- After you have identified inefficient VMs, you will need to add or remove resources to resolve issues.
	There are multiple ways in which you can address inefficient VMs in a cluster.
	
	- If a VM is under-provisioned or constrained, you can hot-plug CPU and memory on the VM while the VM in powered on.
	- If a VM is over-provisioned, you can remove resources from it. However, note that, unlike adding resources, the VM must be powered off if you want to remove resources.
	- You can use Playbooks to automate VM management tasks. For example, you can define the action of adding memory to a constrained VM, or reducing the amount of CPU assigned to an overprovisioned VM.
	- You can use Capacity Runway to view resource runway information and optimize resources that help you improve your cluster's resource allocation or capacity. Based on the current or historical CPU, memory, and storage usage demand, the system may recommend you to:
	    - Address inefficient VMs in your cluster, or
	    - Add a node if you encounter resource constraints in your cluster.

To troubleshoot high CPU in Nutanix environments, you need to identify where and when high CPU utilization occurs. There are three components in a Nutanix infrastructure where you can observe high CPU, each impacting clusters differently.

1. Controller VM (CVM): High CPU utilization in a CVM may impact VMs running in one node or have a cluster-wide impact. 
2. Hypervisor: High CPU utilization in a hypervisor generally impacts the VMs running on that node. 
3. User VMs: High CPU utilization in a user VM will impact only the application or services running in that VM. 

You can find out the cause of the high CPU by answering the following questions:

1. Is the high CPU constant or periodic (random, every hour, every day)? For example, a periodic spike may be attributed to antivirus, backup or similar scheduled tasks being run in the background.
2. Is the high CPU observed on some or all hosts, VMs, CVMs?
3. How long has the high CPU been occurring? If there was a high CPU recently, were there any updates or patches applied recently?
4. Is the sizing right for workloads running? Ensuring that there are sufficient vCPUs assigned to VMs, hosts, and the cluster is important to avoid high CPU usage.

 Information gathering and case framing are crucial aspects of troubleshooting performance. To enhance problem understanding and case framing, consider addressing the following questions: 

1. What are the issues or concerns?
2. What are the performance expectations? Is a certain number of IOPs, throughput level, or latency threshold expected? If a specific test is being performed, what defines success with this test?
3. Is the issue isolated to specific applications, UVMs, vDisks, hosts, or CVMs? If so, which ones (IDs) and where are they located?
4. Is the issue constant or intermittent? 
    1. If intermittent, is it predictable or reproducible?
    2. Is there a time when the issue is more prevalent?
    3. What is the best time window to focus on the issue?
5. What is the business impact of the issue?
6. How and where is the issue being measured?
7. When did the issue start? Was the VM or the application previously running on non-Nutanix hardware, and since the migration to Nutanix, it has been experiencing degraded performance? Is the hardware configuration (CPU, memory, SSD sizes) of before and after comparable?
8. Have any recent changes been made to the environment, including upgrades?
9. Are there any corollary events logged on any participating device? 
10. Are any non-default parameters set?
11. If a load generator or microbenchmark utility, e.g. vdbench, iometer, etc, is being used, what are the testing parameters used. Does this mimic the intended workload for this cluster?