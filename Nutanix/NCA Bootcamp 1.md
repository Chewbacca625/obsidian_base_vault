### Resiliency and Redundancy
- How it works?
	- Oplog exists on each node (SSD) - Persistent Write Buffer
		- When a write comes through the data is routed via the CVM to the local oplog
		- From there based on Replication Factor (RF) 1 original + 1 copy (RF2) or 1 original + 2 copies (RF3) the data will be synchronously replicated to the other nodes oplogs (Other oplogs will acknowledgement once the data is stored on them)
		- From there the data is sequentially drained down into the extent store on the hosts where the original and copies have been stored (on the backend)
	- Benefits:
		- This ensures data protection ensuring we always have a copy of your data
		- 