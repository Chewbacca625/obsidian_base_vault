- Cloud Arch & Design
- Advanced Design Topics
- Enhanced IAM and PKI?
- Networking
- Other Common Cloud Services and Tech
- Cloud Storage
- Cloud Networking Services
- Virtualization 
![[Screenshot 2026-03-12 at 8.42.01 AM.png]]

![[Screenshot 2026-03-12 at 8.41.54 AM.png]]

![[Screenshot 2026-03-12 at 8.41.35 AM.png]]

![[Screenshot 2026-03-12 at 8.41.21 AM.png]]

![[Screenshot 2026-03-12 at 8.41.05 AM.png]]

![[Screenshot 2026-03-12 at 8.40.52 AM.png]]

![[Screenshot 2026-03-12 at 8.40.39 AM.png]]

![[Screenshot 2026-03-12 at 8.40.32 AM.png]]

![[Screenshot 2026-03-12 at 8.40.17 AM.png]]

![[Screenshot 2026-03-12 at 8.40.08 AM.png]]

![[Screenshot 2026-03-12 at 8.39.52 AM.png]]

![[Screenshot 2026-03-12 at 8.39.21 AM.png]]


![[Screenshot 2026-03-12 at 8.39.10 AM.png]]

![[Screenshot 2026-03-12 at 8.39.01 AM.png]]

![[Screenshot 2026-03-12 at 8.38.50 AM.png]]

![[Screenshot 2026-03-12 at 8.38.43 AM.png]]

![[Screenshot 2026-03-12 at 8.38.34 AM.png]]

![[Screenshot 2026-03-12 at 8.38.27 AM.png]]

![[Screenshot 2026-03-12 at 8.38.19 AM.png]]

![[Screenshot 2026-03-12 at 8.38.09 AM.png]]

### Networking Segmentation & Sec Protocols
- SSL/TLS -> backbone of HTTPS - TCP 443
- IPsec is how traffic is tunneled - VPN 
- Services:
	- DHCP
	- DNS
	- IPAM
	- NTP
	- CDN
	- MPLS: traffic is labeled and moved across network layer 2.5 technique
	- VPN - IPsec (authorization, authentication, logging, encryption)
	- SDN
		- Data plane: user traffic speed
		- Control plane: routing, updates, management

### VPN Concepts
- Encryption data helps protect its confidentiality
- VPN tunneling protocols encrypt network traffic before it leaves the source computer and its decrypted at the destination computer.
- Tunneling Protocols:
	- IPsec: Encapsulation protocol that encrypts any type of app layer protocol and is commonly used with VPN solutions
	- SSH: remote admin protocol 
	- Layer 2 Tunneling Protocol (L2TP)/IPsec: A common vpn protocol that relies on IPsec for encryption
	- Point-to-point Tunneling protocol (PPTP): legacy protocol that should be avoided
	- OpenVPN: open source, strong security, slower

### VPN Designs
- Site-to-Site: tunneled connection between two sites (VPN router on each side)
	- HQ to branch
	- Private Data Center to one or more public clouds
- Point-to-Site/Point (P2P): remote access VPN to site
	- WFH to corp site

### Backup Types
- Full backup - take a full backup of the data
	- slowest backup, but simplest
- Incremental backup - take backup of what has changed since the last full backup or incremental backup. (fastest backup, slowest to restore)
	- Backup each day (tuesday will only backup data since last full, wednesday will backup only whats changed since tuesday, etc.)
- Differential backup - take a backup of what has changed since the last full backup.