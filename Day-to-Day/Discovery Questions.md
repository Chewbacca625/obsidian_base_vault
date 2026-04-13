
MISSES: upgrade/firmware (LCM), multivendor (API integrations), introductions
We can integrate with third party vendors api and add those to PC dashboard

### Intros
- what brought you here today?
- I want to hear from you first hand and why you want to explore options with Nutanix?
- my job is to listen to your challenges and see how we could help you?

### Dodge/Redirect
- Thats a really good questions. I haven't heard that one before? How does that feature support/effect your current env?

### Big Picture Discovery
- Do you have a cloud footprint?
- Whats your current DR strategy?
- What is your refresh cycle?
- Do you have newer or older hardware
- What does your current IT environment look like?
- What are your challenges and successes with your current environment?
- What functionality do you seek with cloud adoption?
- What’s the impact of frequent patching downtime on your operations?
- What is your current patching process and where is the bottleneck?
- What is the business impact due to the slow downs, and what is the limitation with your current system resulting in the slowdown?
- What was the business impact of the outage, and what challenges did you face bringing the systems back online?

### Networking

### 1. Performance & Application Delivery

Focus: Identifying if the current network is a bottleneck for end-user experience.

- "When your team scales a new workload or application, what is the current process for provisioning the necessary network resources, and how long does that typically take?"  
- "Have there been instances where storage traffic and VM traffic competed for bandwidth, and how did that impact your application's 'time-to-glass' for the end user?"  
- "What is your current strategy for handling 'east-west' traffic (server-to-server) as your virtualized environment grows?"  

### 2. Reliability & Redundancy

Focus: Quantifying the cost of downtime and the robustness of the physical layer.

- "In the event of a Top-of-Rack switch failure, what is the automated failover path for your storage traffic, and has that been tested recently?" 
- "What is the business impact if the management plane of your network goes down while the data plane remains active?"  
- "Can your current 10GbE (or 25GbE) infrastructure handle the low-latency requirements of a distributed storage fabric without introducing dropped packets?"  

### 3. Security & Microsegmentation

Focus: Assessing the risk of lateral movement and the complexity of security rules.

- "If a single virtual machine in your environment were compromised tomorrow, what automated controls are in place to prevent that threat from moving laterally to your core database?"  
- "How much time does your team spend manually updating firewall rules or VLAN tags when a server is moved, or a new department is onboarded?"  
- "Are there specific compliance or regulatory requirements (like HIPAA or PCI) that require strict isolation of your network traffic?"  
### 4. Future Growth & Operations

Focus: Aligning network capabilities with the customer’s long-term cloud strategy.

- "As you look toward a hybrid cloud model, how are you planning to maintain consistent networking and security policies between your on-premises datacenter and the public cloud?"  
- "What visibility do you currently have into the 'dark corners' of your network—specifically, how do you troubleshoot latency between a specific VM and its storage?"


> Focus on Impact -> Outcome

