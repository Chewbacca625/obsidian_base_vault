Vsphere - PE
Vcenter - Prism
ESXi - AHV
Vsan - Software solution for storage similar to DSF


Sizing notes
- Why is a random cluster carved out?
- Are they currently using kubernetes?
	- I saw a few docker vms? Are they using openshift???
- Vdisk - possible SAN is it virtual or physical???
- No 10Gbits no good lol
- CPU is in MHZ and when max of 2.5GHz is 2500 Mhz x each core = 25000 Mhz if there are 10
- total mhz is how much compute you not really vcpu or cores.
  intel 10 core 2.6GHz= 26000MHz
  AMD 10 core 3.4GHz = 34000 MHz - more performance same license cost
### Sizing Lesson
- What projects, apps, scaling do you have down the line (6months ect.)
- We prioritize right sizing
- salesforce login is for acutal account deals
- mynutanix and salesforce are connected but its easier to use salesforce
- normalized cores: how many acual cores you actually running virually (spec int ratio) - AHV/VMWARE (Virtual Slicing and scheduling)
- assuming RF2 (two copies of data look at ram, HDD, and cpu those are changing because data is backuped) cuts into raw remaining capacity
- notate notate notate - protects you
- typically you will create 3 sizers 
- most look at storage and CPU
- dont let sales people do discovery 
- live optics, rvtools, nutanix collector!!! VITAL we should not be sizing on the fly
- we need to know your workloads, we have to see names or IP addrs (can be anonymous), how much storage, what your current hardware looks like, switches and nics, nic speeds, ram have they have vs use, CPU are physically, ratio core to vcpu
- typically 6:1 but we would do 4:1 with a raw sizing
- its an unvalidated solution without a collectors file
Deal reg
a - approved - cost is caculated based on the account deal
d- not approved - cost isnt accurate???
Discovery questions
- ask if you have ups's or power support high voltage is required...
- it can run on one power supply but its not made to run on one
- fill out scenario objectives 
