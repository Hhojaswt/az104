- Configure storage accounts

You can think of Azure Storage as supporting three categories of data: structured data, unstructured data, and virtual machine data. Review the following categories and think about which types of storage are used in your organization.
Expand table
Category	Description	Storage examples
Virtual machine data	Virtual machine data storage includes disks and files. Disks are persistent block storage for Azure IaaS virtual machines. Files are fully managed file shares in the cloud.	Storage for virtual machine data is provided through Azure managed disks. Data disks are used by virtual machines to store data like database files, website static content, or custom application code. The number of data disks you can add depends on the virtual machine size. Each data disk has a maximum capacity of 32,767 GB.
Unstructured data	Unstructured data is the least organized. The format of unstructured data is referred to as nonrelational.	Unstructured data can be stored by using Azure Blob Storage and Azure Data Lake Storage. Blob Storage is a highly scalable, REST-based cloud object store. Azure Data Lake Storage is the Hadoop Distributed File System (HDFS) as a service.
Structured data	Structured data is stored in a relational format that has a shared schema. Structured data is often contained in a database table with rows, columns, and keys. Tables are an autoscaling NoSQL store.	Structured data can be stored by using Azure Table Storage, Azure Cosmos DB, and Azure SQL Database. Azure Cosmos DB is a globally distributed database service. Azure SQL Database is a fully managed database-as-a-service built on SQL.

Storage account types
General purpose Azure storage accounts have two types: Standard and Premium.
	• Standard storage accounts are backed by magnetic hard disk drives (HDD). A standard storage account provides the lowest cost per GB. You can use Standard storage for applications that require bulk storage or where data is infrequently accessed.
	• Premium storage accounts are backed by solid-state drives (SSD) and offer consistent low-latency performance. You can use Premium storage for Azure virtual machine disks with I/O-intensive applications like databases.
	
	无法将标准存储帐户转换为高级存储帐户，反之亦然。 必须使用所需的类型创建新的存储帐户，并将数据（如果适用）复制到新的存储帐户。
	
	
	Azure Storage offers four data services that can be accessed by using an Azure storage account:
		• Azure Blob Storage (containers): A massively scalable object store for text and binary data.
		• Azure Files: Managed file shares for cloud or on-premises deployments.
		• Azure Queue Storage: A messaging store for reliable messaging between application components.
		 队列消息最大可以为 64 KB，一个队列可以包含数百万条消息。
		• Azure Table Storage: A service that stores nonrelational structured data (also known as structured NoSQL data).
		
		Storage account	Supported services	Recommended usage
		Standard general-purpose v2	Blob Storage (including Data Lake Storage), Queue Storage, Table Storage, and Azure Files	Standard storage account for most scenarios, including blobs, file shares, queues, tables, and disks (page blobs).
		Premium block blobs	Blob Storage (including Data Lake Storage)	Premium storage account for block blobs and append blobs. Recommended for applications with high transaction rates. Use Premium block blobs if you work with smaller objects or require consistently low storage latency. This storage is designed to scale with your applications.
		Premium file shares	Azure Files	Premium storage account for file shares only. Recommended for enterprise or high-performance scale applications. Use Premium file shares if you require support for both Server Message Block (SMB) and NFS file shares.
		Premium page blobs	Page blobs only	Premium high-performance storage account for page blobs only. Page blobs are ideal for storing index-based and sparse data structures, such as operating systems, data disks for virtual machines, and databases.
		
Replication ensures your storage account meets the Service-Level Agreement (SLA) for Azure Storage even if there are failures.
We explore four replication strategies:
	• Locally redundant storage (LRS)
	• Zone redundant storage (ZRS)
	• Geo-redundant storage (GRS)
	• Geo-zone-redundant storage (GZRS)
	• 本地冗余存储 (LRS) 在主要区域中的单个物理位置同步复制数据三次。 LRS 是成本最低的复制选项，但不建议对需要高可用性或持续性的应用程序使用此选项。
	• 区域冗余存储 (ZRS) 跨主要区域中的三个 Azure 可用性区域同步复制数据。 对于需要高可用性的应用程序，Microsoft 建议在主要区域中使用 ZRS，并复制到次要区域。
Azure 存储提供了两种将数据复制到次要区域的选项：
	• 异地冗余存储 (GRS) 使用 LRS 在主区域中的单个物理位置同步复制数据三次。 然后，它将数据异步复制到次要区域中的单个物理位置。 在次要区域内，数据使用 LRS 同步复制了 3 次。
	• 异地区域冗余存储 (GZRS) 使用 ZRS 在主区域中的三个 Azure 可用性区域同步复制数据。 然后，它将数据异步复制到次要区域中的单个物理位置。 在次要区域内，数据使用 LRS 同步复制了 3 次。
	
Locally redundant storage is the lowest-cost replication option and offers the least durability compared to other strategies.

Zone redundant storage synchronously replicates your data across three storage clusters in a single region.

我们创建 URL 来访问存储帐户中的某个对象:



- Configure azure blob storage
Blob Storage can store any type of text or binary data.
Some examples are text documents, images, video files, and application installers.

Blob Storage uses three resources to store and manage your data:
• An Azure storage account
• Containers in an Azure storage account
• Blobs in a container

A blob can't exist by itself in Blob Storage. A blob must be stored in a container resource.

Configure a container
Name: Enter a name for your container. The name must be unique within the Azure storage account.

container data is private and visible only to the account owner. There are three access level choices:
• 专用：（默认）禁止匿名访问容器和 Blob。
• Blob：仅允许对 Blob 进行匿名公共读取访问。
• 容器：对整个容器（包括 Blob）的匿名公共读取和列表访问。



- Configure azure storage security
• Encryption at rest. Storage Service Encryption (SSE) with a 256-bit Advanced Encryption Standard (AES) cipher encrypts all data written to Azure Storage. When you read data from Azure Storage, Azure Storage decrypts the data before returning it. This process incurs no extra charges and doesn't degrade performance. It can't be disabled.
 所有写入 Azure 存储的数据都由具有 256 位高级加密标准 (AES) 密码的存储服务加密 (SSE) 进行加密。

A shared access signature (SAS) is a uniform resource identifier (URI) that grants restricted access rights to Azure Storage resources. SAS is a secure way to share your storage resources without compromising your account keys.

共享访问签名 (SAS) 是一种统一资源标识符 (URI)，可授予对 Azure 存储资源的受限访问权限。 SAS 可安全地共享存储资源，而不会危及帐户密钥。


创建共享访问签名 (SAS) 时，使用参数和令牌创建统一资源标识符 (URI)。 该 URI 由 Azure 存储资源 URI 和 SAS 令牌组成。

• 数据在写入 Azure 存储之前会自动加密。
• 检索数据时，会自动对其进行解密。
• Azure 存储加密、静态加密、解密和密钥管理对用户来说都是透明的。
• 写入 Azure 存储的所有数据均通过 256 位高级加密标准 (AES) 加密进行加密。 AES 是可用的最强分组加密技术之一。
• Azure 存储加密会针对所有新的和现有的存储帐户启用，并且不能禁用。


- Configure Azure Files
可以通过使用服务器消息块 (SMB)、网络文件系统 (NFS) 和 HTTP 协议来访问 Azure 文件共享。
You can access Azure file shares by using the Server Message Block (SMB), Network File System (NFS), and HTTP protocols. 


• 无服务器部署。 Azure 文件共享是完全托管的文件共享的 PaaS 产品/服务，不需要任何基础结构。 你无需处理任何 VM、操作系统或更新。
• 存储空间几乎无限制。 单个 Azure 文件共享最多可存储 100 TiB 的文件，文件的大小最大可达 4 TiB。 这些文件在分层文件夹结构中的组织方式与在本地文件服务器上的组织方式相同。
• 数据加密。 Azure 文件共享上的数据存储在 Azure 数据中心并在网络上传输时，会静态加密。
• 从任何位置访问。 默认情况下，如果客户端具有 Internet 连接，则可以从任何位置访问 Azure 文件共享。
• 集成到现有环境中。 可以使用 Microsoft Entra 标识或已同步到 Microsoft Entra ID 的 AD DS 标识来控制对 Azure 文件共享的访问。 这有助于确保用户访问 Azure 文件共享的体验与访问本地文件服务器时的体验相同。
• 先前版本和备份。 你可以创建与文件资源管理器中的“先前版本”功能集成的 Azure 文件共享快照。 还可以使用 Azure 备份来备份 Azure 文件共享。
• 数据冗余。 Azure 文件共享数据将复制到同一 Azure 数据中心的多个位置或多个 Azure 数据中心。 包含文件共享的 Azure 存储帐户的复制设置可控制数据冗余。


Azure Files supports two storage tiers: premium and standard. Standard file shares are created in general purpose (GPv2) storage accounts, while premium file shares are created in FileStorage storage accounts. 

Premium	Premium file shares store data on solid-state drives (SSDs), and are available only in the FileStorage storage account kind. They provide consistent high performance and low latency, and are available in LRS redundancy, with ZRS available in some regions. Not available in all Azure regions.
Standard	Standard file shares store data on hard disk drives (HDDs) and deploy in the general-purpose version 2 (GPv2) storage account type. Provide performance for workloads such as general-purpose file shares and dev/test environments. Standard file shares are available for LRS, ZRS, GRS, and GZRS, in all Azure regions.




Creating SMB Azure file shares
Two important things: 1. Open port 445 2. Enable secure transfer


• Soft delete for file shares is enabled at the file share level.
• Soft delete transitions content to a soft deleted state instead of being permanently erased.
• Soft delete lets you configure the retention period. The retention period is the amount of time that soft deleted file shares are stored and available for recovery.
• Soft delete provides a retention period between 1 and 365 days.
• Soft delete can be enabled on either new or existing file shares.

Azure Storage Explorer lets you connect to different storage accounts.
• Connect to storage accounts associated with your Azure subscriptions.
• Connect to storage accounts and services that are shared from other Azure subscriptions.
• Connect to and manage local storage by using the Azure Storage Emulator.

Let's take a look at the characteristics of Azure File Sync.
	• Azure File Sync transforms Windows Server into a quick cache of your Azure Files shares.
	• You can use any protocol that's available on Windows Server to access your data locally with Azure File Sync, including SMB, NFS, and FTPS.
	• Azure File Sync supports as many caches as you need around the world.
	
	Cloud tiering is an optional feature of Azure File Sync. Frequently accessed files are cached locally on the server while all other files are tiered to Azure Files based on policy settings.
	


