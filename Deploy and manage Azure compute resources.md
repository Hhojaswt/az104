- Introduction to Azure virtual machines

Environment	dev, prod, QA	Identifies the environment for the resource
Location	eus for East US, jw for Japan West	Identifies the region into which the resource is deployed
Instance	01, 02	For resources that have more than one named instance (web servers, etc.)
Product or Service	service	Identifies the product, application, or service that the resource supports
Role	sql, web, messaging	Identifies the role of the associated resource
First, the location can limit your available options. Each region has different hardware available and some configurations aren't available in all regions. Second, there are price differences between locations.

The VM size can be changed while the VM is running, as long as the new size is available in the current hardware cluster the VM is running on. The Azure portal makes the size options obvious by only showing you available size choices. The command line tools report an error if you attempt to resize a VM to an unavailable size. Changing a running VM size automatically reboots the machine to complete the request.

Pay as you go	With the pay-as-you-go option, you pay for compute capacity by the second, with no long-term commitment or upfront payments. You're able to increase or decrease compute capacity on demand and start or stop at any time. Select this option if you run applications with short-term or unpredictable workloads that can't be interrupted. For example, if you're doing a quick test, or developing an app in a VM, pay-as-you-go is the appropriate option.
Reserved Virtual Machine Instances	The Reserved Virtual Machine Instances (RI) option is an advance purchase of a virtual machine for one or three years in a specified region. The commitment is made up front, and in return, you get up to 72% price savings compared to pay-as-you-go pricing. RIs are flexible and can easily be exchanged or returned for an early termination fee. Select this option if the VM has to run continuously, or you need budget predictability, and you can commit to using the VM for at least a year.

All Azure virtual machines have at least two virtual hard disks (VHDs). The first disk stores the operating system, and the second is used as temporary storage. 

 Azure Site Recovery is about replication of virtual or physical machines; it keeps your workloads available in an outage.
While there are many attractive technical features to Site Recovery, there are at least two significant business advantages:
	• Site Recovery enables the use of Azure as a destination for recovery, thus eliminating the cost and complexity of maintaining a secondary physical datacenter.
	• Site Recovery makes it incredibly simple to test failovers for recovery drills without impacting production environments. This feature makes it easy to test your planned or 
	




- Configure virtual machine availability
An availability plan for Azure virtual machines needs to include strategies for unplanned hardware maintenance, unexpected downtime, and planned maintenance.
	• An unplanned hardware maintenance event occurs when the Azure platform predicts that the hardware or any platform component associated to a physical machine is about to fail. When the platform predicts a failure, it issues an unplanned hardware maintenance event. Azure uses Live Migration technology to migrate your virtual machines from the failing hardware to a healthy physical machine. Live Migration is a virtual machine preserving operation that only pauses the virtual machine for a short time, but performance might be reduced before or after the event.
	• Unexpected downtime occurs when the hardware or the physical infrastructure for your virtual machine fails unexpectedly. Unexpected downtime can include local network failures, local disk failures, or other rack level failures. When detected, the Azure platform automatically migrates (heals) your virtual machine to a healthy physical machine in the same datacenter. During the healing procedure, virtual machines experience downtime (reboot) and in some cases loss of the temporary drive.
	• Planned maintenance events are periodic updates made by Microsoft to the underlying Azure platform to improve overall reliability, performance, and security of the platform infrastructure that your virtual machines run on.
	
	
	An update domain is a group of nodes that are upgraded together during the process of a service upgrade (or roll out). An update domain allows Azure to perform incremental or rolling upgrades across a deployment. Here are some other characteristics of update domains.
		• Each update domain contains a set of virtual machines and associated physical hardware that can be updated and rebooted at the same time.
		• During planned maintenance, only one update domain is rebooted at a time.
		• By default, there are five (non-user-configurable) update domains.
		• You can configure up to 20 update domains.
		
		
		Virtual machines in the same fault domain share a common power source and physical network switch.
		
		Review the following characteristics of availability zones.
			• Availability zones are unique physical locations within an Azure region.
			• Each zone is made up of one or more datacenters that are equipped with independent power, cooling, and networking.
			• To ensure resiliency, there's a minimum of three separate zones in all enabled regions.
			• The physical separation of availability zones within a region protects applications and data from datacenter failures.
			• Zone-redundant services replicate your applications and data across availability zones to protect against single-points-of-failure.
			Category	Description	Examples
			Zonal services	Azure zonal services pin each resource to a specific zone.	- Azure Virtual Machines
					- Azure managed disks
					- Standard IP addresses
			Zone-redundant services	For Azure services that are zone-redundant, the platform replicates automatically across all zones.	- Azure Storage that's zone-redundant
					- Azure SQL Database
		
	Vertical scaling makes a virtual machine more (scale up) or less (scale down) powerful.
	
	When you implement horizontal scaling, there's an increase (scale out) or decrease (scale in) in the number of virtual machine instances.
	
	With Virtual Machine Scale Sets, you don't need to pre-provision your virtual machines. It's easier to build large-scale services that target large compute, big data, and containerized workloads.
	
	• All virtual machine instances are created from the same base operating system image and configuration. This approach lets you easily manage hundreds of virtual machines without extra configuration tasks or network management.
	• Virtual Machine Scale Sets support the use of Azure Load Balancer for basic layer-4 traffic distribution, and Azure Application Gateway for more advanced layer-7 traffic distribution and TLS/SSL termination.
	• You can use Virtual Machine Scale Sets to run multiple instances of your application. If one of the virtual machine instances has a problem, customers continue to access your application through another virtual machine instance with minimal interruption.
	• Customer demand for your application might change throughout the day or week. To meet customer demand, Virtual Machine Scale Sets implements autoscaling to automatically increase and decrease the number of virtual machines.
	• Virtual Machine Scale Sets support up to 1,000 virtual machine instances. If you create and upload your own custom virtual machine images, the limit is 600 virtual machine instances.
	
	

	
	
	- Configure Azure App Service plans
The pricing tiers are grouped into three categories:
	Shared compute:
	• Free and Shared, the two base tiers, run an app on the same Azure VM as other App Service apps, including apps of other customers.
	• These tiers allocate CPU quotas to each app that runs on the shared resources, and the resources can't scale out.
	• These tiers are intended to be used only for development and testing purposes.
	Dedicated compute:
	• The Basic, Standard, Premium, PremiumV2, and PremiumV3 tiers run apps on dedicated Azure VMs.
	• Only apps in the same App Service plan have the same compute resources. The higher the tier, the more VM instances that are available to you for scale-out.
	Isolated:
	• The Isolated and IsolatedV2 tiers run dedicated Azure VMs on dedicated Azure virtual networks.
	• This tier provides network isolation on top of compute isolation to your apps.
	• This tier provides the maximum scale-out capabilities.
	
	
	• The scale up method increases the amount of CPU, memory, and disk space. Scaling up gives you extra features like dedicated virtual machines, custom domains and certificates, staging slots, autoscaling, and more. You scale up by changing the pricing tier of the Azure App Service plan where your application is placed.
	• The scale-out method increases the number of virtual machine instances that run your application. You can scale out to as many as 30 instances, depending on your App Service plan pricing tier. Take advantage of App Service Environments in the Isolated tier to further increase your scale-out count to 100 instances. The scale instance count can be configured manually or automatically (autoscale).
	• With autoscale, you can automatically increase the scale instance count for the scale-out method. Autoscale is based on predefined rules and schedules.
	• Your App Service plan can be scaled up and down at any time by changing the pricing tier of the plan.
	
	

	
- Implement Azure App Service
• Always On: You can keep your app loaded even when there's no traffic. This setting is required for continuous WebJobs or for WebJobs that are triggered by using a CRON expression.
• Session affinity: In a multi-instance deployment, you can ensure your app client is routed to the same instance for the life of the session.
• HTTPS Only: When enabled, all HTTP traffic is redirected to HTTPS.

Continuous deployment (CI/CD) is a process used to push out new features and bug fixes in a fast and repetitive pattern with minimal impact on end users. Azure supports automated deployment directly from several sources:
	• GitHub: Azure supports automated deployment directly from GitHub. When you connect your GitHub repository to Azure for automated deployment, any changes you push to your production branch on GitHub are automatically deployed for you.
	• Bitbucket: With its similarities to GitHub, you can configure an automated deployment with Bitbucket.
	• Local Git: The App Service Web Apps feature offers a local URL that you can add as a repository.
	• Azure Repos: Azure Repos is a set of version control tools that you can use to manage your code. Whether your software project is large or small, using version control as soon as possible is a good idea.
	
	Swapped settings	Slot-specific settings
	Language stack and version, 32/64-bit	Custom domain names
	App settings *	Nonpublic certificates and TLS/SSL settings
	Connection strings *	Scale settings
	Mounted storage accounts*	Always On
	Public certificates	IP restrictions
	WebJobs content	WebJobs schedulers
	Hybrid connections **	Diagnostic settings
	Service endpoints **	Cross-origin resource sharing (CORS)
	Azure Content Delivery Network **	Virtual network integration
	Path mapping	Managed identities
	• Allow Anonymous requests (no action). Defer authorization of unauthenticated traffic to your application code. For authenticated requests, App Service also passes along authentication information in the HTTP headers. This feature provides more flexibility for handling anonymous requests. With this feature, you can present multiple sign-in providers to your users.
	• Allow only authenticated requests. Redirect all anonymous requests to /.auth/login/<provider> for the provider you choose. The feature is equivalent to Log in with <provider>. If the anonymous request comes from a native mobile app, the returned response is an HTTP 401 Unauthorized message. With this feature, you don't need to write any authentication code in your app.
	




- Configure containers to virtual machines
Compare	Containers	Virtual machines
Isolation	A container typically provides lightweight isolation from the host and other containers, but a container doesn't provide as strong a security boundary as a virtual machine.	A virtual machine provides complete isolation from the host operating system and other virtual machines. This separation is useful when a strong security boundary is critical, such as hosting apps from competing companies on the same server or cluster.
Operating system	Containers run the user mode portion of an operating system and can be tailored to contain just the needed services for your app. This approach helps you use fewer system resources.	Virtual machines run a complete operating system including the kernel, which requires more system resources (CPU, memory, and storage).
Deployment	You can deploy individual containers by using Docker via the command line. You can deploy multiple containers by using an orchestrator such as Azure Kubernetes Service.	You can deploy individual virtual machines by using Windows Admin Center or Hyper-V Manager. You can deploy multiple virtual machines by using PowerShell or System Center Virtual Machine Manager.
Persistent storage	Containers use Azure Disks for local storage for a single node, or Azure Files (SMB shares) for storage shared by multiple nodes or servers.	Virtual machines use a virtual hard disk (VHD) for local storage for a single machine, or an SMB file share for storage shared by multiple servers.
Fault tolerance	If a cluster node fails, the orchestrator on another cluster node rapidly recreates any containers running on the node.	Virtual machines can fail over to another server in a cluster, where the virtual machine's operating system restarts on the new server.
