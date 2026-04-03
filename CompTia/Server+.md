### Server Components
- Multiprocessor
	- Current offerings: Both use LGA (Land Grid Array Sockets-flat bottoms (no pins))
		- Xeon Intel
		- Epyc AMD
- Virtualization softwares:
	- Intel VT
	- AMD V
- Ram:
	- ECC Mem: Detects and corrects single bit data corruption in real-time
	- DDR: Double-Data Rate (xxxx = speed)
		1. DDR = PC-xxxx standard | 184 pins
		2. DDR2 = PC2-xxxx standard | 240 pins
		3. DDR3 = PC3-xxxx standard | 240 pins 
		4. DDR4 = PC4-xxxx standard | 288 pins
			-  capacity larger
			- voltage reduced over ddr3
		5. DDR5 = PC5-xxxx standard | 288 pins
- PCIe:
	- 64-bit
	- Full Duplex, serial communication
	- Multilink (x1-x16)
	- Versions:
		- 1.0 = x1 250 MBps
		- 2.0 = x1 500 MBps
		- 3.0 = x1 GBps
		- 4.0 = x1 2 GBps
		- 5.0 = x1 4 GBps
- USB: Serial Comm, 5V power, various peripherals
	- 2.0 480 Mbps
	- 3.2 Gen 1 5Gbps
	- 3.2 Gen 2 10 Gbps
	- 3.2 Gen 2.2 20 Gbps
	- 4 40 Gbps
### Networking Cabling and Components
- Twisted Pair Cables:
	- UTP: Unshielded Twisted Pair
	- STP: Shielded Twisted Pair - 10Gbps +
	- Categories:
		- CAT5: 100 Mbps | 100 Meters
		- CAT5e: 1Gbps | 100 Meters
		- CAT6: 10 Gbps | 55 Meters
		- CAT6A: 1Gbps & 10Gbps | 100 Meters
		- CAT7: 1 Gbps and 10 Gbps | 100 Meters
	- RJ45 : 8P8C connector
- Fiber Optic Cabling:
	- Single-Mode Fiber Optic Cabling
		- Greatest distance of bounded media
		- Core diameter 9 microns
		- Cladding diameter: 125 microns
		- Uses Lazer
	- Multimode Fiber Optic Cabling: shorter distances than single mode fiber
		- Greatest distance of bounded media
		- Core diameter 50 or 62.5 microns
		- Cladding diameter: 125 microns
		- Uses LED source
		![[Screenshot 2026-03-09 at 5.24.46 PM.png]]
	- Transivers for Fiber Optic:
		- SFP - Small form factor pluggable (Hot Pluggable | Greater Port Density) 100Mbps, 1 Gbps | Gigabit Eth
		- SFP+ up to 16 Gbps | 8Gbps Fiber Channel (GFC)/16 GFC, 10 GigEth
		- QSFP 4 channels | 4 Gbps (1 x 4) | 4GFC Gigabit Eth DDR InfiniBand (Double Data Rate
		- QSFP+ 4 channels | 40 Gbps (10 x 4) | 10 Gbps Eth, 40 Gbps Eth, 10 GFC FiberChannel, QDR InfiniBand
### Server Racks and Power
- Heaviest on the bottom of rack
- Cable trays clean up cables
- Cable management arm allow for cables to not get pulled out
- PDU: Power Distribution Unit - Serge Protector | Can be managed Remotely
- UPS: Uninterruptible Power Supply - Power Supply | Online (help with power spikes)/Offline (Running or Not)

### Storage Drive and Interface Types
- Storage Types:
	- SSD
	- NVMe Storage
	- SAS SCSI (Small Computer System Interface ) HDD
	- PCIe Server SDD
	- PCIe Express NVMe SSD
	- Reads/Writes are what degrade SSDs (Mean time between failure)
	- Hybrid Drives: Cold data stored on disk, flash commonly accessed data (Don't last as long?)
	- SD Card
- Interfaces:
	- Serial ATA = Power + Data
	- eSATA older version of usb/thunderbolt
	- SAS SCSI (Serial Attached SCSI SAS): reliable and performant server storage interface
- SD Cards:
	- SDSC: 2 GB Fat 16
	- SDHC: 2-32GB Fat32
	- SDXC: 32GB-1TB exFat
	- SDUC: 2TB-128TB exFat
- SATA Interfaces:
	- 1.0 150 MBps
	- 2.0 300 MBps
	- 3.0 600 MBps
- SAS Interfaces:
	- SAS-1 3 Gbps
	- SAS-2 6 Gbps
	- SAS-3 12 Gbps
	- SAS-4 22.5 Gbps

### Shared Storage
- NAS - Network Attached Storage Device - Typically Eth based (TCP/IP) | Slower | Not Scalable
- SAN - Storage Area Network - dedicated high speed network that connects server to consolidated block level storage devices. Typically uses dedicated Fiber Channel Network pairing switches, storage arrays, and servers interface with HBA (Host Bus Adapter) using SCSI based communication to connect to fiber channel network that connects directly to storage arrays. 
	- Highly scalable
	- Very Expensive
	- Proprietary Technology (Very Specialized)
	- Can be implemented via Eth using TCP/IP (Slower) - to route fiber channel over eth we need Internet SCSI (iSCSI)
- File Systems:
	- NFS - Network file system - client server protocol, distributed file system
	- SMB - Sever Message Block - resource and file sharing on windows based networks
### Server Fault Tolerance & Capacity
- Fault tolerance: We need to be able withstand the failure of hardware not if but when it happens
- Redundancy: Achieved with fault tolerance
- RAID:
	- RAID 0: Block Level Striping (min 2 disks) | 0 redundancy | Speeds up data storage
		![[Screenshot 2026-03-09 at 6.48.47 PM.png]]
	- RAID 1: Mirroring (min 2 disks) | 1 Redundancy | Loose 50% of storage
		- Raid 1 with duplexing: two raid controllers for redundancy
		![[Screenshot 2026-03-09 at 6.49.51 PM.png|625]]
	- RAID 5: Striping with parity | Min 3 drives | Hot Swappable and parity will rebuild array | 3tb - 1 parity data = 2 tb data | 1 Redundancy 
		![[Screenshot 2026-03-09 at 6.52.16 PM.png]]
	- RAID 6: Striping w/ Double Parity | min 4 drives
		![[Pasted image 20260309185451.png]]
	- RAID 10 (1+0): Striping + Mirroring | min 4 drives
		![[Screenshot 2026-03-09 at 6.56.52 PM.png]]
	- JBOD (Just a bunch of disks): Large storage pool
	- Hardware based RAID: Dedicated RAID Controller | best performance
	- Software based RAID: Software based RAID Controller | Slower
### Server Management & Maintenance
- KVM (Keyboard/Video/Mouse) - I/O for server mgmt
	- KVM-Switch - Mgmt over TCP/IP
- Windows Admin Center: Cloud like remote mgmt interface
- vSphere: Admin Console to manage vmware compute
- Hot-Swappable Components: Drive, PSU, ETC
	- Mostly on prem
	- Hot Spares: Drives plugged in that can be act as failovers when a drive fails
- RDP: remote desktop management
- Out-of-band mgmt: dedicated network connection (ILO (HPE), iDrac (Dell)) | hardware level access | supports remote mgmt regardless if corporate network goes down