Script:
- Why Kubernetes?
	- 
- Challenges with deploying kuberentes
	- https://landscape.cncf.io/?group=projects-and-products
	- https://www.gartner.com/en/documents/5361263
- How Nutanix makes kuberentes easier?
	- Nutanix Selling Points
- Demo
	- Interface
	- How to manage kuberentes cluster
	- Deploy cool app in NKP to show
- Closing remarks (reiterate how nutanix makes kuberentes easy)
	- If you're ready to adopt enterprise Kuberentes the easy way, lets schedule a follow up!
	- Follow up link

### Notes
- By 2027, 90% of global organizations will be running containerized applications in production vs <40% in 2021
- Majority (56%) of containers deployed require persistent storage
- Emerging workloads (AI and data heavy workloads) drive container usage
- 92% agree that dev should not be managing kub infra
	- Wastes time
	- create complexity
	- wastes money
- 86% of those using VMs and Containers want a unified platform
	- inefficent and expensive
	- security risk
	- defeats the purpose of virtualization
- number 1 challenge of adopting kuberentes is cost 
- 50% now run kuberentes in production at the edge, driven by AI workloads
- its often misunderstood that kuberentes itself is not production ready
- Kubernetes is great at orchestrating, but it lacks a lot of vital functionality so the CNCF was built around solving these problems that it doesnt solve itself ex security, sso, networking, observability, etc.
- 12000 products exist already, we hand pick the best and built a production ready Kubernetes platform
- We ship pure upsteam opensource kuberentes, its all based on the CNCF, and supports freedom of choice
- manages all of these components as a single platform, and its deployed all in one installation
	- in an upgrade we manage all the components updates
		- no need to pay a commercial vendors
		- none of the self management overhead like when one of the projects has a vunderability, or you deploy a new componet or update and it requires extensive testing, you get to focus on what matters most your applications (time, money, effort)
		- we handle the testing and how each component works
- Built on best-of-breed open source like DIY
- Fully supported like other enterprise platforms
- Easy to use like the public cloud
- Supported, tested, automated unlike DIY
- No lock-in unlike other enterprise platforms
- Complete and open platform unlike public cloud

- Tanzu is a non starter
	- very expensive
	- not adopted well
- OpenShift
	- 2-4x more expensive
	- more complex
	- weeks to deploy
	- proprietary pieces (meant to be open source)
		- Dev tools
		- multi cluster fleet management
		- domestic support centers extra cost
		- day 2 odf propritary
		- observability data fabric propritary
		- Redhat os only
- rancher
	- rancher is cheaper
	- support is expensive
	- suse linux diffrent lisencing model
	- much cheaper
		- not as feature rich - we have day 2 service (files,objects,NDB) and a more complete ecosystem
		- only have a dozen of services
		- lifecycle management is difficult
			- NKP is very robust, far more feature rich
			- we include ubuntu pro support
			- lifecycle management
			- you dont have to run it on nutanix, it can run anywhere (platform agnostic, hardware today no interdependency)
- NKP Downsides
	- none really we focus on using rich open source tools
	- projects are segregated so we need to click through the UI, or too much capability
	- insights inside NKP too much security, etc. that presented CVEs all that can be overwhelming, multi cloud management (production and enterprise ready)
	- apps are updated for you so you can focus on your apps
	- you can onboard rancher, eks, azure kub, etc. openshift and tanzu proprietary 

- ASK about multicloud management
	- Multi cloud managmenet leads to locking, you need to use there storage, db, etc.
	- You get the control plane you have to manage the monitoring, etc.


So when we login to NKP we are greeted with this dashboard with information regarding our kuberentes estate. The goal of NKP is again to provide one unified platform to standardize the way you manage your kubernetes in the entirety of your fleet, no mater where your cluster exist may it be in public clouds or on prem.

First, when we drop into our cluster management interface no mater if its on prem, in a public cloud, or the edge this is where we can manage our clusters individually. You can see where its deployed here. The great thing is you can see observe, enforce, and build your kubernetes operating polices from one place. It looks and feels the same no matter if its on prem or in a public cloud. You avoid the nuance of managing various kubernetes enviornments. Our cluster runtime extensions allow us to dismantle the proprietary pieces of the public clouds and gain the benifits that come with them.

You can deploy a cluster by clicking up here on add cluster, we can attach any CNCF compliant kuberentes clusters that may exist in your enviornment already or create a new nutanix cluster via create cluster here though the UI by defining a few project variables, so you can get you cluster online in minutes or hours rather than weeks or months.

When I drop into our management cluster, you can see all of your open source kuberentes management apps ready to go for the picking. For example we can hop into grafana we can monitor our cluster immediately no need to configure or hook things up its ready to go out of the box. This applies to workload cluster you deploy as well.

On the infrastrucure provider tab here, we can add external connectivity with other providers like AWS, Azure, Vsphere to automate cluster provisioning and handle infrastructure lifecycle management.

Lastly, we prioritize integrations for SSO and security. You can setup your identity provider of choice here and you can manage role based access conrtol for your nutanix kubernetes platform here under access control. We have a few built in roles that you can assign to users or groups who require them.

When we log in to NKP, we're greeted with a dashboard that gives us a view of our entire Kubernetes estate. Again the goal of NKP is to provide one unified platform that standardizes how you manage your full Kubernetes fleet — whether your clusters live in public clouds, on-prem, or at the edge.

Starting with cluster management: this is where you manage your clusters individually, regardless of where they're deployed. From one place, you can observe, enforce, and build your Kubernetes operating policies — and the experience looks and feels the same whether you're on-prem or in a public cloud. This eliminates the friction of managing multiple Kubernetes environments. Our cluster runtime extensions let you shed the proprietary constraints of public clouds while still capturing the benefits that come with them.

To deploy a cluster, click **Add Cluster** up here. You can attach any CNCF-compliant Kubernetes cluster that already exists in your environment, or create a new Nutanix cluster directly through the UI by defining a few variables. Either way, you're getting clusters online in minutes or hours — not weeks or months.

Dropping into our management cluster, you'll see a full suite of open-source Kubernetes management apps, ready to use out of the box. Take Grafana as an example — you can jump straight into monitoring your cluster with no configuration or setup required. The same applies to any workload clusters you deploy too.

On the **Infrastructure Provider** tab, you can add external connectivity with providers like AWS, Azure, or vSphere to automate cluster provisioning and manage infrastructure lifecycle.

Finally, security and integrations are a first-class priority. You can configure your identity provider of choice for SSO, and manage role-based access control for your Nutanix Kubernetes Platform under **Access Control** — with several built-in roles ready to assign to users or groups as needed.



