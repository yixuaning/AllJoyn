#### Proxy bus object 
[37IOT物联网开发社区](http://37iot.com)是国内专业的物联网开发技术论坛，欢迎各位有趣之士进入共同进步。

AllJoyn总线上的总线对象通过代理(Proxies)访问。代理是一个远程对象的本地表示,通过总线访问。代理是一种常见的术语，不是特定于AllJoyn系统，但是你会经常遇到ProxyBusObject一词，在AllJoyn框架中指出proxy的具体对象，它是一个位于远程对象的本地代理总线。 

ProxyBusObject 是底层(low-level)AllJoyn代码的一部分。它使能一个代理对象的基本对象。

通常，一个RMI系统的目标是提供一个代理，实现了一个接口，看起来像是一个我们可以调用的远程对象。The proxy object implements the same interface as the remote object, but drives the process of marshaling the parameters and sending the data to the service.

在AllJoyn框架，客户端和服务软件，往往通过特定的编程语言绑定，提供实际的用户级代理对象。这个用户级代理对象使用AllJoyn代理总线对象(AllJoyn proxy bus object)的能力去实现他本地/远程透明的目标。

#### Bus names 
一个AllJoyn总线上的连接作为服务提供接口的实现，通过接口名称来描述。接口的实现在服务中组成接口对象的一个树。客户端希望通过代理对象(proxy objects)使用服务,使用底层的AllJoyn代理总线对象通过逻辑总线对象去安排总线方法、总线信号和总线所有权等相关信息的交付。

为了完成总线寻址，对总线连接的名称必须是唯一的。AllJoyn为每个总线附属(bus attachment)提供一个唯一的临时总线名称。然而，这个唯一的名字是每次服务服务连接到总线自动产生的，因此不适合作为一个持久的服务标识符。必须有一个一致的和持久的方式让服务附属到总线上。
这些持久的名字被称作 well-known names.

正如一台主机系统在互联网上的域名不会随时间变化(e.g., 37iot.com)，调用一个AllJoyn总线上的功能部件通过它well-known的总线名称。正如接口名称似乎是反向的域名，总线名字有相同的格式。请注意，这里会产生一些混淆，因为接口名称和well-known总线名称为了方便起见时常选择相同的字符串。记住他们代表不同的目的，接口名称标识客户端和服务之间的合同,由总线对象实现，驻守在总线附属(bus attachment)中；well-known名称标识服务和客户端通过一致的方式连接到那个附属(attachment)。

使用一个well-known名字，一个应用(通过总线连接)必须向总线Router使用这个名字。如果well-known名字没有被另外一个应用使用，同意独家使用这个well-known名字。这就是为什么在任何时间，well-known名字在总线上保证代表唯一的地址。

通常,一个well-known名字意味着约定，相关的总线连接实现了一组总线对象，因此可用服务的一些概念。由于总线名称提供了一个在分布式总线上唯一的地址，他们在总线中必须是唯一的。例如，可以使用总线名称,**org.alljoyn.sample.chat**，这将表明一个同名的总线连接可以实现聊天服务。由于用了这个名字,人们可以推断,它在一个总线对象的**/org/alljoyn/sample/chat**目录实现了相应的**org.alljoyn.sample.chat**接口

这个问题是为了“聊天”,人们会期望看到AllJoyn总线上的另一个类似的组件表明它支持聊天服务。由于总线名称必须唯一地标识一个总线连接，这就需要某种形式的后缀来保证唯一性。这可能需要一个用户名或一个唯一的编号的形式。在聊天的例子中,可以想象多个总线连接:

> org.alljoyn.sample.chat.bob 

> org.alljoyn.sample.chat.carol

在这种情况下，well-known名称前缀**org.alljoyn.sample.chat.**充当服务名称，从中可以推断聊天接口和对象实现的存在。后缀**bob**和**carol**用来让well-known名称保持唯一。

这导致的问题如何服务位于分布式系统。答案是通过服务去广告和发现客户端。

#### Advertisements and discovery
服务的广告和发现有两方面的问题。如上所述，即使服务驻留在本地的AllJoyn总线，人们需要能够看到并检查总线上的所有总线连接的well-known名称，为了确定其中一个感兴趣的特定服务。一个更有趣的问题是考虑如何发现服务不属于现有的总线段。

考虑将一个运行AllJoyn框架的设备接近另一个会发生什么。由于两个设备物理上一直是分开的。这两个总线Routers没有办法获得任何其他知识。如何确定其他节点的存在，他们是如何确定这里是否有设备需要连接到其他设备来形成一个逻辑分布式AllJoyn总线？

答案是通过AllJoyn服务广告和发现设备。当一个设服务开始在一个本地设备上运行，它储备一个well-known的名字,然后广告它的存在给附近的其他设备。AllJoyn框架提供了一个抽象层来让服务可以执行广告操作，这可能是通过底层技术透明的沟通。如Wi-Fi，Wi-Fi直接，或其他/未来的无线传输。无论是客户端还是服务都不需要了解这些广告是如何由底层技术管理。

例如，在一个contacts-exchanging应用中，一个应用会储存**org.alljoyn.sample.contacts.bob**作为well-known名字，并广告这个名字。这可能导致一个或多个以下：一个已经连接的Wi-Fi下的UDP组播，一个Wi-Fi直连的pre-association服务的广告，或蓝牙服务发现协议的消息。广告是如何沟通的机制，发送广告的人不一定要关注。由于contacts-exchange应用从概念上讲是一个peer-too-peer应用，人们都期待第二个电话也是用相似的服务，例如，**org.alljoyn.sample.contacts.carol**.

客户机应用程序可能会宣布他们发现接收广告的兴趣上来操作。客户端应用程序可以通过初始化一个发现操作来宣布他们对接收广告有兴趣。例如，它可能要求发现前缀是**org.alljoyn.sample.contacts**的服务。 In this case, both devices would make that request.

只要手机接近对方，底层的AllJoyn系统在可用的传输上发送和接收广告。每次都会自动的接收一个指出相应的服务是可用的。

因为服务广告可以通过多个传输接收，并在某些情况下，需要附加底层的工作提出一个潜在的通信机制，这是使用发现服务的另外一个概念部分。这是通信会话。

#### Sessions
总线的概念名称、对象路径和接口名称已经讨论过了。回想一下，当一个实体连接到一个AllJoyn总线，它被分配一个唯一的名称。总线连接(bus attachments)会要求他们有一个well-known名称。Well-known名称是用来客户端在总线上定位或发现服务的。例如，一个服务连接到一个AllJoyn总线并被总线分配了唯一的名字**:1.1**,。如果服务想让总线上的其他实体能够找到它，服务必须在总线上请求一个well-known名称，例如，**com.companyA.ProductA**(记住，一个唯一的实例限定符通常会附加在后面)。

这个名字意味着至少一个总线对象，它实现了一些有意义的Well-known接口。通常，总线对象认为连接实例内的具有相同组件的路径作为Well-Known名称(这不是必需的,它只是一个惯例)。在这个例子中，总线名称**com.companyA.ProductA**相对应的总线对象路径可能是**/com/companyA/ProductA**。

In order to understand how a communication session from a client bus attachment to a similar service attachment is formed and to provide an end-to-end example, it is useful to compare and contrast the AllJoyn mechanism to a more familiar mechanism.

Postal address analogy
在AllJoyn框架中,服务请求一个人类可读的名字,所以可以用一个well-known和well-understond标签广告自己。Well-known名称必须翻译成唯一的名字，为了底层网络正确的路由信息，例如:

> Well-known-name:org.alljoyn.sample.chat 

> Unique name::1.1

这告诉我们，Well-known名称用**org.alljoyn.sample.chat**广告，相当于总线连接已经分配了唯一的标识符**:1.1**。一个能想到的使用这种方式的是一个公司有一个名称和一个通信地址。继续类比，一个常见的情况是一个企业与其他企业座落在同一个建筑。在这种情况下，可能会发现一个业务地址被房间号码进一步限定。由于AllJoyn总线连接可以提供多个服务，还必须有一种方法来识别多个目标在一个特定的连接。一个"contact port number"对应于通讯地址类比中的房间号。​

Just as one may send a letter by the national mail system (U.S. Post Office, La Poste Suisse) or a private company (Federal Express, United Parcel Service) and by different urgencies (overnight, two-day, overland delivery), when contacting a service using the AllJoyn framework, one must specify certain desired characteristics of the network connection to provide a complete delivery specification (e.g., reliably delivered messages, reliably delivered unstructured data, or unreliably delivered unstructured data).

Notice the separation of the address information and the delivery information in the example above. Just as one can contemplate choosing several ways to get a letter from one place to another, it will become evident that one can choose from several ways to get data delivered using the AllJoyn system.

#### The AllJoyn session
就像一个正确标签的邮政信件有“从”和”到“地址，一个AllJoyn会话需要等效的"from"和"to"信息。至于AllJoyn系统，from 地址相当于客户端组件的地址，to 地址则涉及到服务。​

从技术上讲，这些from/to地址，在计算机网络中，被称为 half-associations。在AllJoyn框架，to地址(服务地址)有以下形式：
​{session options, bus name, session port}​

第一个字段："session options"涉及到如何将数据从一个连接到另一个。在一个IP网络,可以选择TCP或UDP。在AllJoyn框架，这些细节可能是抽象的，所以选择"message-based","unstructured data"或"unreliable unstructured data"。目标服务用well-known名称指定相应的总线连接请求。​

类似房间号在通讯地址中的例子,the AllJoyn model has the concept of a point of delivery "inside" the bus attachment.在AllJoyn框架中,这被称为一个会话。正如房间号只在一个给定的建筑中有意义,会话端口号只有总线连接的范围内有意义。The existence and values of contact ports are inferred from the bus name in the same way that underlying collections of objects and interfaces are inferred.

The from address, corresponding to the client information, is similarly formed. A client must have its own half-association in order to communicate with the service.

> {session options, unique name, session ID}

It is not required for clients to request a well-known bus name, so they provide their unique name (such as **:1.1**). Since clients do not act as the destination of a session, they do not provide a session port, but are assigned a session ID when the connection is established. Also during the session establishment procedure, a session ID is returned to the service. For those familiar with TCP networking, this is equivalent to the connection establishment procedure used in TCP, where the service is contacted over a well-known port. When the connection is established, the client uses an ephemeral port to describe a similar half-association.

During the session establishment procedure, the two half-associations are effectively joined:

> {session options, bus name, session port}    Service

> {session options, unique name, session ID}    Client

Notice that there are two instances of the session options. When communication establishment begins, these may be viewed as supported session options provided by the service and requested session options provided by the client. Part of the session establishment procedure consists of negotiating an actual final set of options to be used in the session. Once a session has been formed, the half-associations of the client and service side describe a unique AllJoyn communication path:

> ​{session options, bus name, unique name, session ID}

During the session establishment procedure, a logical networking connection is formed between the communicating routing nodes. This may result in the creation of a wireless radio topology management operation. If such a connection already exists, it is re-used. A newly created underlying router-to-router connection is used to perform initial security checks, and once this is complete, the two routers have effectively joined the two separate AllJoyn software bus segments into the larger virtual bus.

Because issues regarding end-to-end flow control of the underlying connection must be balanced with topological concerns in some technologies, the actual connection between the two communicating endpoints (the "from" client and the "to" service) may or may not result in a separate communication channel being formed. In some cases it is better to flow messages over an ad hoc topology and in some cases it may be better to flow messages directly over a new connection (TCP/IP). This is another of the situations that may require deep understanding of the underlying technology to resolve, and which the AllJoyn framework happily accomplishes for you. A user need only be aware that messages are routed correctly over a transport mechanism that meets the abstract needs of the application.

​
