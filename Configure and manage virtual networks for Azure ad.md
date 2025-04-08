- Configure virtual networks
• Each subnet contains a range of IP addresses that fall within the virtual network address space.
• The address range for a subnet must be unique within the address space for the virtual network.
• 一个子网的范围不能与同一虚拟网络中的其他子网 IP 地址范围重叠。
• 子网的 IP 地址空间必须使用 CIDR 表示法指定。
• 可以在 Azure 门户中将虚拟网络划分为一个或多个子网。 子网的 IP 地址特征已列出。


For each subnet, Azure reserves five IP addresses. The first four addresses and the last address are reserved.
Let's examine the reserved addresses in an IP address range of 192.168.1.0/24.


接下来，我们将详细介绍 IP 地址的特征。
	• 可以静态分配或动态分配 IP 地址。
	• 可以将动态和静态分配的 IP 资源分离到不同的子网中。
	• 静态 IP 地址不会更改，在某些情况下最为适合，例如：
	• DNS 名称解析，其中 IP 地址更改时需更新主机记录。
	• 基于 IP 地址的安全模型，它们要求应用或服务具有静态 IP 地址。
	• 链接到 IP 地址的 TLS/SSL 证书。
	• 使用 IP 地址范围允许或拒绝流量的防火墙规则。
	• 基于角色的虚拟机，例如域控制器和 DNS 服务器。

如果选择 IPv6 作为“IP 版本”，则对于基本 SKU，分配方法必须为“动态”。 对于 IPv4 和 IPv6 地址，标准 SKU 地址均为“静态”。


可以根据资源所部署到的虚拟网络子网的地址范围来分配专用 IP 地址。 有两个选项：动态和静态。
	• 动态：Azure 会分配子网的地址范围内下一个未分配或未保留的可用 IP 地址。 动态分配为默认分配方法。
假设地址 10.0.0.4 到 10.0.0.9 已分配给其他资源。 在这种情况下，Azure 会将地址 10.0.0.10 分配给新资源。
	• 静态：选择并分配子网的地址范围内任何未分配或未保留的 IP 地址。
假设子网的地址范围是 10.0.0.0/16，地址 10.0.0.4 到 10.0.0.9 已分配给其他资源。 在此方案中，可以分配介于 10.0.0.10 和 10.0.255.254 之间的任何地址。





- Configure network security groups
Network security groups are a way to limit network traffic to resources in your virtual network.

可以通过为以下任何设置指定条件来向网络安全组添加更多安全规则：
• Name
• Priority
• 端口
• 协议（任何、TCP、UDP）
• 源（任何、IP 地址、服务标记）
• 目标（任何、IP 地址、虚拟网络）
• 操作（允许或拒绝）

• 对于入站流量，Azure 会首先处理任何关联子网的网络安全组安全规则，然后再处理任何关联的网络接口。
• 对于出站流量，进程是反向的。 Azure 首先评估任何关联网络接口的网络安全组安全规则，然后再评估任何关联的子网。


入站流量有效规则
Azure 会处理配置中所有 VM 的入站流量规则。 Azure 会确定 VM 是否是 NSG 的成员，以及它们是否具有关联的子网或 NIC。
	• 创建 NSG 时，Azure 会为组创建默认安全规则 DenyAllInbound。 默认行为是拒绝 Internet 的所有入站流量。 如果 NSG 具有子网或 NIC，则子网或 NIC 的规则可以替代默认的 Azure 安全规则。
	• VM 中子网的 NSG 入站规则优先于同一 VM 中 NIC 的 NSG 入站规则。
出站流量有效规则
Azure 通过首先检查所有 VM 中 NIC 的 NSG 关联来处理出站流量的规则。
	• 创建 NSG 时，Azure 会为组创建默认安全规则 AllowInternetOutbound。 默认行为是允许流向 Internet 的所有出站流量。 如果 NSG 具有子网或 NIC，则子网或 NIC 的规则可以替代默认的 Azure 安全规则。
	• VM 中 NIC 的 NSG 出站规则优先于同一 VM 中子网的 NSG 出站规则。
	
	可以在 Azure 虚拟网络中实现应用程序安全组，以按工作负载对虚拟机进行逻辑分组。 然后，可以根据应用程序安全组定义网络安全组规则。
	有关使用应用程序安全组的注意事项
	应用程序安全组的工作方式与网络安全组相同，但它们提供了一种以应用程序为中心的方法来查看基础结构。 将虚拟机加入应用程序安全组。 然后，使用应用程序安全组作为网络安全组规则中的源或目标。
	
	如果检查 wideworldimports.com 的 DNS 区域，你会看到两条顶点域记录：NS 和 SOA。 创建 DNS 区域时，会自动创建 NS 和 SOA 记录。
	
	
	What is an apex domain?
	如果检查 wideworldimports.com 的 DNS 区域，你会看到两条顶点域记录：NS 和 SOA。 创建 DNS 区域时，会自动创建 NS 和 SOA 记录。
	
	Azure alias records enable a zone apex domain to reference other Azure resources from the DNS zone. You don't need to create complex redirection policies. 
	
	别名记录允许你将区域顶点 (wideworldimports.com) 链接到负载均衡器。 
	



- Azure DNS

Azure DNS lets you host your Domain Name System (DNS) records for your domains on Azure infrastructure. 

下面简要概述了 DNS 服务器解析域名查找请求时使用的过程：
	• 如果域名存储在短期缓存中，DNS服务器就会解析域请求。
	• 如果域不在缓存中，它将联系 Web 上的一个或多个 DNS 服务器，以查看它们是否匹配。 找到匹配项后，DNS 服务器将更新本地缓存并解析请求。
	• 如果在合理数量的 DNS 检查后找不到该域，DNS 服务器将返回“找不到域”错误。

• “A”是主机记录，也是最常见的 DNS 记录类型。 它将域或主机名映射到 IP 地址。
• CNAME 是一条规范名称记录，用于从一个域名创建别名以将其映射到另一个域名。 如果你具有所有访问同一网站的不同域名，则将使用 CNAME。
• “MX”是邮件交换记录。 它将邮件请求映射到邮件服务器，无论是在本地还是在云中托管。
• “TXT”是文本记录。 它用于将文本字符串与域名相关联。 Azure 和 Microsoft 365 使用 TXT 记录来验证域所有权。

下一步是验证委托域现在是否指向为该域创建的 Azure DNS 区域。 此过程可能需要 10 分钟，也可能需要更长时间。
若要验证域委托是否成功，可以查询颁发机构起始 (SOA) 记录。 设置 Azure DNS 区域时自动创建了 SOA 记录。 可以使用 nslookup 等工具验证 SOA 记录。




- Configure Azure Virtual network peering
• Regional virtual network peering connects Azure virtual networks that exist in the same region.
• Global virtual network peering connects Azure virtual networks that exist in different regions.
• 一个虚拟网络只能有一个 VPN 网关。


- Configure virtual networks
Subnet 是对 IP 地址范围（CIDR）的一种逻辑划分，它把一个大的网络（比如 VNet）切分成多个较小的段，用于组织、隔离和控制网络流量。


网关是一种网络设备或服务，用于连接两个不同网络，并进行协议转换、路由转发或访问控制。
它可以是一个物理设备（如路由器、防火墙），也可以是一个软件或云端服务（如 Azure VPN Gateway、Application Gateway）。





- Manage and control traffic flow in your Azure deployment with routes
路由 = 网络流量的导航规则表，而不是某个具体的设备。




虚拟网络服务终结点（Virtual Network Service Endpoints）
✔️ 是什么？
它让你在 Azure 的专用虚拟网络中直接访问 Azure 的服务（比如存储账号），而不需要通过公网 IP。
✔️ 有啥用？
	• 提高安全性：虚拟机访问 Azure 服务时不会走公网，只能走内部网络。
	• 拒绝公共网络访问：比如你有个 Storage Account，只允许内部访问，别人即使有密钥也无法从公网访问它。

🚦 自定义路由（Custom Routes）
✔️ 是什么？
默认情况下，Azure 会自动生成路由让虚拟机可以联网。但有时候我们希望更精细地控制流量，比如：
	• 指定某些流量必须走某台 NVA（网络虚拟设备，比如防火墙）
	• 不让虚拟机直接上网
	• 所有出站流量必须走安全检查
这种时候就用 自定义路由。

🧭 用户定义路由（User-Defined Routes，UDR）
✔️ 是什么？
它是自定义路由的一种方式，让你自己写路由规则控制流量走向。
✔️ 举个例子：
假设你有两个子网，一个是 Web 子网，一个是防火墙子网。你可以写个 UDR，让 Web 子网的出站流量先经过防火墙子网再出去。



🛣 路由（Routing / Route）
	• 是一个 规则表，规定了去往某个目的 IP 地址的流量应该怎么走。
	• 在 Azure、Linux、Cisco 等系统中，都是一张表，比如：

scss
CopyEdit
目的网络       子网掩码       下一跳
10.1.0.0       255.255.255.0   192.168.1.1
0.0.0.0        0.0.0.0         10.0.0.1 (默认网关)
	• 路由表本身不会“发包”，它只是提供决策参考。
🚪 网关（Gateway）
	• 是一个网络节点，专门用于连接不同网络（比如：内部网络 和 互联网）。
	• 你的设备如果要访问另一个子网，或者访问公网，必须通过网关出去。
	• 常见的类型有：
		○ 默认网关（Default Gateway）
		○ NAT 网关
		○ VPN 网关
		○ ExpressRoute 网关（Azure 中专用线路）

如果多个路由具有同一地址前缀，则 Azure 会根据其类型按以下优先级顺序选择路由：
	1. 用户定义路由
	2. BGP 路由
	3. 系统路由
	
	网络虚拟设备 (NVA) 是一种虚拟设备，由各种层构成，例如：NVA 就是防火墙，比如 Palo Alto、Fortinet、Check Point、Azure Firewall。
		防火墙
		WAN 优化器
		应用程序交付控制器
		路由器
		负载均衡器
		IDS/IPS
		代理
		




- Introduction to load balancer
用来自动将进入的流量分发到后端的多个 VM 或服务实例上

🧱 Azure 提供两种类型的负载均衡器：
1. 公共负载均衡器（Public Load Balancer）
	• 有公网 IP，面向 Internet。
	• 适合网站、API 等需要从外部访问的服务。
	• 举例：一个 Web 应用放在 3 台 VM 上，公共负载均衡器把所有用户请求平均分发给这 3 台 VM。
2. 内部负载均衡器（Internal Load Balancer）
	• 没有公网 IP，只在虚拟网络内部使用。
	• 用于服务之间的调用，比如：
		○ Web 层访问业务层
		○ 应用访问数据库层
		○ 微服务之间的内部通信


协议	解释	特点	场景举例
TCP	传输控制协议	可靠、有序、确认机制，慢但稳定	网页浏览、文件传输、邮件、SSH、数据库
UDP	用户数据报协议	不可靠、无连接、无确认，快但可能丢包	视频通话、直播、DNS 查询、在线游戏
🔁 类比：
	• TCP 就像顺丰快递：每一件都签收确认、顺序正确、安全送达。
	• UDP 就像扔传单：扔了就扔了，快，但可能有丢失、顺序错乱。

场景	描述
在虚拟网络中	仅用于虚拟网络内部的 VM 之间流量分发
跨区域 VNet	多区域部署时，在同一个虚拟网络中通过负载均衡器协调流量
多层应用架构	Web → 业务层 → 数据层，每一层都可能用内部负载均衡
LOB 应用（Line-of-Business）	某些企业核心应用只限企业内部用户访问，不需要暴露公网


🧠 什么是“前端 IP”？
前端 IP（Frontend IP）就是用户访问你服务时所使用的入口地址。
	• 可以是 公网 IP 👉 让 Internet 用户访问
	• 也可以是 专用 IP（私网 IP） 👉 只允许内部系统访问
你可以为一个 Azure Load Balancer 设置一个或多个前端 IP。


⚙️ 例子：

用户 → 公网 IP:80 → Azure Public Load Balancer → Web VM
你部署的网站接收到的公网 IP 请求，其实是 Azure LB 收到的，然后再转发给后端的 VM。





🧪 第一部分：运行状况探测（Health Probe）
目的：
用来判断后端 VM 实例是否健康（能不能正常处理请求）。不健康就自动停止转发请求到该实例。
🔍 运行状况探测类型：
类型	说明
TCP 探测	尝试在指定端口建立 TCP 连接，连得上就算健康。
HTTP/HTTPS 探测	发一个 HTTP/HTTPS 请求（比如访问 /healthcheck），响应 200 就是健康。
⚠️ 如果 VM 应用崩了、关机了、延迟超时了，探测就会失败。



🔁 第二部分：会话暂停（Session Persistence）
目的：
控制同一个用户的请求是否一直落在同一台后端 VM 上，提高用户体验。
💡 配置选项：
模式	说明
无（默认）	每次请求都可能打到不同的 VM，流量随机分配。
客户端 IP	每个客户端的请求始终被路由到相同的 VM。
客户端 IP + 协议	进一步细分，不同协议的请求分开对待。
适用于有状态应用，比如登录态、购物车等。

🔐 第三部分：高可用性端口（HA Ports）
目的：
用于负载均衡所有端口、所有协议的流量，特别适合部署防火墙类设备（NVA），不需要为每个端口写规则。
⚙️ 特点：
	• 只适用于标准 SKU 的 Load Balancer
	• 负载均衡规则设置为：

makefile
CopyEdit
Frontend: Port All
Backend: Port All
Protocol: All
💡 这样一条规则就可以转发所有 TCP/UDP 流量，无需逐个端口手动配置。

🌐 第四部分：入站 NAT 规则（Inbound NAT Rule）
目的：
用于把负载均衡器前端 IP 的某个端口映射到 VM 的某个端口，实现远程访问（如 RDP/SSH）。
🖥 举例：
	• 公网 IP 的 3389 端口 → 映射到 VM1 的 3389 端口（远程桌面）
	• 公网 IP 的 22 端口 → 映射到 VM2 的 22 端口（SSH）
⚠️ NAT 规则是点对点的映射，不做负载均衡。


📦 OSI 七层模型（网络通信的分层结构）
OSI（Open Systems Interconnection）模型是网络通信的理论模型，把网络分为七层，每层负责一部分任务。这样做的目的是：
	✨ 分层解耦，职责清晰，易于设计和排查网络问题。

✅ 七层模型（从上到下）：
层级	名称	作用	举例
7️⃣	应用层 (Application)	提供给用户使用的服务	HTTP、HTTPS、FTP、SMTP、DNS
6️⃣	表示层 (Presentation)	数据编码/加密/解码	TLS/SSL、JPEG、MP3
5️⃣	会话层 (Session)	建立、维护、终止会话	NetBIOS、RPC
4️⃣	传输层 (Transport)	端口、可靠性、数据流控制	TCP、UDP
3️⃣	网络层 (Network)	IP 地址、路径选择（路由）	IP、ICMP、路由协议（BGP）
2️⃣	数据链路层 (Data Link)	MAC 地址、以太网帧	以太网（Ethernet）、ARP
1️⃣	物理层 (Physical)	电压、光信号、网线	网线、光纤、无线信号

💡 类比：你快递寄文件的过程（现实比喻）
层	名称	类比解释
7 应用层	你写的信件内容（要说啥）	
6 表示层	把信件翻译成收件人看得懂的语言（编码/加密）	
5 会话层	和快递公司打电话建好派送计划（建立会话）	
4 传输层	拿到运单，确认路线、是否需要签收（TCP/UDP）	
3 网络层	快递员看地址怎么走（IP 路由）	
2 数据链路层	包裹在物流仓内部传递（MAC 地址）	
1 物理层	快递车运送、走高速、飞机（电信号、光信号）	


你想做的操作包括两个目标：
	1. ✅ 不再把新连接分配到这台 VM 上（防止新会话进来）
	2. ✅ 允许已有连接自然完成（避免中断用户请求）
这正是 Azure Application Gateway 的 “Connection Draining”（连接排出）功能 的作用。

🔧 Connection Draining（连接排出）详解：
功能	描述
✅ 停止新请求	一旦你从后端池中手动移除某台 VM，它将不再接收新连接
✅ 等待旧连接结束	在你配置的“排出时间”内（例如 30 秒、60 秒），现有连接仍然保持，直到自然关闭
✅ 无缝下线	对用户无感知，零中断下线后端节点，适合灰度升级、打补丁等维护操作






- Introduction to Azure Application Gateway
Azure 应用程序网关（Application Gateway） 是 Azure 提供的一种 第 7 层（应用层）负载均衡器，专门处理 HTTP、HTTPS 流量，它不仅做转发，还可以：
	• 检查 URL、主机头、路径等 HTTP 内容
	• 实现 Web 防火墙（WAF）
	• 做 SSL 卸载、Cookie 粘性、URL 重写等高级功能
	• 适合处理 Web 应用的前端请求（比如网站、API 网关）
	
	功能对比	Application Gateway	Azure Load Balancer
	OSI 层级	第 7 层（应用层）	第 4 层（传输层）
	协议支持	HTTP / HTTPS / WebSocket / HTTP/2	TCP / UDP
	是否能看 HTTP 内容	✅ 可以（能根据 URL 路由）	❌ 不行（只看 IP + 端口）
	能否做 WAF 防护	✅ 支持 Web 应用防火墙（WAF）	❌ 不支持
	常见用途	网站、Web API、反向代理、安全控制	游戏服务、数据库、任何 TCP/UDP 应用
	是否支持 SSL 卸载	✅ 支持	❌ 不支持
	是否支持 URL 路由、重定向	✅ 支持	❌ 不支持
	性能	相对慢一些（功能多）	高性能、低延迟

	✅ Application Gateway（App Gateway）到底干了啥？
	它其实既是“网关”又是“负载均衡器”，两种功能融合在一起，甚至还能防火墙：
	能力	有/没有	举例说明
	流量入口（网关）	✅ 有	接收来自 Internet 或 VNet 的流量
	流量转发（转发器）	✅ 有	把请求根据规则转发到不同后端池
	负载均衡	✅ 有	会把多个请求分配到多个后端 VM
	应用层智能判断	✅ 有	能根据 URL 路由 / Cookie 粘性做分发
	安全控制	✅ 有	集成 Web 应用防火墙（WAF）
	
	🚦 和普通网关的区别：
	普通的“网关”（比如 VPN Gateway、NAT Gateway）只是流量通道，不参与转发判断、负载均衡。
	但 Application Gateway 是“聪明的网关”：
	✅ 它知道你访问的是 /login 还是 /images
	✅ 它知道你是不是用 HTTPS
	✅ 它知道哪个后端 VM 最近压力小，自动分流
	✅ 它甚至可以拒绝某些非法请求（WAF）
	
	🧱 关键组件讲解：
	组件	作用
	前端 IP 配置（Frontend IP）	接收客户端的请求，可以是公网 IP 或内网 IP
	监听器（HTTP/HTTPS Listener）	监听指定端口（如 443），识别进入的 HTTP 或 HTTPS 流量
	HTTP 设置（HTTP Settings）	控制后端通信方式，比如是否使用 HTTPS 与后端通信、Cookie 粘性设置等
	规则（Rule）	把监听器和后端资源关联起来，决定哪些流量走哪个后端池
	Web 应用防火墙（WAF）	提供安全防护，防止常见 Web 攻击（SQL 注入、XSS 等）
	后端池（Backend Pool）	实际承载业务的服务：VM、虚拟机规模集、App Service 等
	
	🔹 一、什么是后端池（Backend Pool）？
	后端池就是一组接收网关转发流量的服务器，可能是：
		• VM（虚拟机）
		• VMSS（虚拟机规模集）
		• Azure App Service
		• 本地服务器（通过 VPN/ExpressRoute）
		• 容器等
	这些服务器是 Application Gateway 流量的最终目的地。







- Azure network watcher
网络观察程序由三组主要的工具和功能组成：
	• 监视
	• 拓扑
	• 连接监视器
	• 网络诊断
	• IP 流验证
	• NSG 诊断
	• 下一跃点
	• 有效安全规则
	• 连接故障排除
	• 数据包捕获
	• VPN 故障排除
	• 交通
	• 流日志
	• 流量分析


✅ 拓扑 是什么？
在 Azure 网络监控中，“拓扑（Topology）”是一种可视化网络结构图，它会展示出你虚拟网络中所有资源之间的连接关系，比如：
	• 哪些子网包含了哪些 VM
	• 哪些 VM 连着哪个 NSG、安全组
	• 哪些资源挂在 Load Balancer、App Gateway、VPN Gateway 上
	• 路由、连接、网关如何转发流量

🧠 通俗理解：
你可以把“拓扑”理解为：
	“把你整个 Azure 网络环境画成一张图，一目了然地看到资源之间是怎么连的。”
它就像你在画 Visio 网络图一样，但 Azure 自动帮你生成，而且是实时的、动态的。

