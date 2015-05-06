## Core Framework  
这一节介绍AllJoyn™核心概念(AllJoyn™ Core concepts)。建议任何人开发AllJoyn应用都能高层次的理解(A base high-level understanding)，即使应用只是使用AllJoyn服务框架。
 
[37IOT物联网开发社区](http://37iot.com)是国内专业的物联网开发技术论坛，欢迎各位有趣之士进入共同进步。

### Bus Attachment 
AllJoyn应用使用和与AllJoyn网络交互必须要在初始化一个AllJoyn总线附属对象(AllJoyn Bus Attachment object)并且将这个对象连接到AllJoyn路由(AllJoyn Router)后。 

### Advertisement and Discovery 
AllJoyn应用可以广告他的服务通过两种机制：About Announcements and Well-Known Name。根据现有的传输(transports)，AllJoyn框架将使用不同的机制来确保应用程序可以由其他AllJoyn应用发现。 对于基于IP的传输，使用mDNS和多路广播与广播UDP包的组合。

**About Announcements** 是被推荐使用的广告机制。它提供了通用的的方法去广播一组一致的应用感兴趣的元数据，如，模型，支持的接口，图形化的图标，以及更多。

**Well-Known Name** 对于应用去公布和发现对方是一种更底层的机制。它是About Announcements使用的机制。推荐应用使用About Announcements机制，除非这里对这种低等级(lower-level)的功能有特别的需要。

在这两种情况下，发现的进程返回一个Alljoyn应用通过独特的名称来区别的列表。这个值用于随后创建进一步的通信会话。

[Learn more about About Announcements. ](https://allseenalliance.org/developers/learn/core/about-announcement)

### Session and Port 
AllJoyn框架负责创建不同AllJoyn应用之间的连接。通常，一个应用会提供一个服务通过About公告(About Announcements)来广告自己。远程应用发现这个应用(和他的UniqueName)，可以创建一个会话，这个过程称作JoinSession。该应用提供的服务有接受或者拒绝JoinSession请求的选项。

会话可以是point-to-point或者multi-point。point-to-point会话容许one-to-one连接，而multi-point容许一组设备/应用在同一个会话中交流。

会话是在一个特定的端口创建。不同的端口容许各种各样的point-to-point和multi-point连接的拓扑结构。在下面的图中，左边，A和B都会point-to-point连接到S的端口1上。右边，A,B,C和S都连接在S的端口2的multi-point会话上。
 ![alljoyn-core-sessions](https://allseenalliance.org/sites/default/files/developers/learn/alljoyn-core-sessions.png)
 
### BusObject
AllJoyn应用互相通讯通过BusObject抽象。这种抽象的地图(abstraction maps)和面向对象的编程思想一样，通过已经定义好的接口创建一个对象。通常，应用提供的服务创建一个BusObject。远程应用可以有效地远程打开这个BusObject然后在它上面调用方法，类似于远程过程调用(remote procedure call).

一个BusObject可以实现一组接口。每个接口清楚地定义了一组BusMethods,BusProperties, and BusSignals。BusMethods容许远程实体调用方法。BusProperties可以被获取和设置。BusSignals是应用提供的服务所发出的的信号。

BusObject附属在一个特别的总线路径上。这容许更大的灵活性，同一个对象可以为不同的目的而附属在不同的总线路径上。例如，如果应用为一个灶台实现了一个服务，StoveBurner BusObject能够连接到多个总线路径比如'/range/left', '/range/right' 能够被创建去控制个别火炉燃烧器。

ProxyBusObject是远程应用创建的对象来获取BusObject权限。

总结，应用提供服务，实现了一个BusObject去公开访问他的服务。远程应用程序创建此应用的会话，并在一个特定的对象路径通过创建ProxyBusObject来连接到它的BusObject。然后调用BusMethods，访问BusProperties,接收BusSignals。
 ![alljoyn-core-busobject](https://allseenalliance.org/sites/default/files/developers/learn/alljoyn-core-busobject.png)
 
### Sessionless Signal
Sessionless Signal是用来无需创建一个会话来接收信号的机制。Under the hood,它使用Well-Known Name广告一个新的信号的存在。远程实体自动创建一个临时会话，接收到信号数据，然后会话会解除。AllJoyn Core APIs提供了这种抽象。

### Introspection
Introspection建立在AllJoyn框架。APIs exist to easily introspect a remote AllJoyn application to discover its object paths and objects; its full interface including all methods; and parameters, properties, and signals. Via introspection, one can learn about the remote device and communicate with it without needing prior information about that device.

### Events and Actions
事件和动作是一个应用程序来描述它的事件和行为的公约。通过为信号和方法添加简单的元数据描述，其他应用程序可以很容易地发现AllJoyn应用会发出什么样的行动和该应用可以接受哪些事件。这就容许其他应用在不同设备上一起动态连接事件和行为，去创建更加复杂的if-this-then-that交互。
[Learn more about Events and Actions.](https://allseenalliance.org/developers/learn/core/events-and-actions)

### Security
AllJoyn security发生在应用层；这里不信任设备级。每个接口可以选择需要security。如果需要，认证发生在两个应用程序之间的方法被调用或接收信号的时候。多种认证机制支持：PIN code, PSK, or ECDSA (Elliptical Curve Digital Signature Algorithm)。一旦通过验证，这两个设备之间的所有信息都使用AES-128 CMM加密。

### Putting It All Together
AllJoyn应用通过总线连接与AllJoyn框架交互。应用通过About公告广告他的服务，列出应用的元数据，包括支持的接口。UniqueName返回发现识别的应用。

当远程应用程序发现AllJoyn应用，它可以通过连接到一个特定的端口创建一个会话。point-to-point 和 multi-point 会议的支持。AllJoyn应用有权接受或拒绝连接请求。

会话创建之前，应用程序可以创建任意数量的总线对象，放在特定的对象路径。每个总线对象可以实现一组接口，由一组方法，属性，和信号定义。

会话创建后，远程应用程序通常会通过创建本地ProxyBusObject与BusObject交互，包括调用方法，获取和设置属性，接收信号。​
 ![alljoyn-core-components](https://allseenalliance.org/sites/default/files/developers/learn/alljoyn-core-components.png)
 
在许多情况下，用户端发现，建立会话和代理对象的管理遵循一个简单的，在应用中常见的模式。标准核心库用[Observer](https://allseenalliance.org/developers/develop/api-guide/core/observer/) class为这种情况提供了一个便利的API。The Observer class自动解析About公告，会话管理和为用户应用创建代理对象(Proxy object)

### Learn more
* [Learn more about the AllJoyn Standard Core](https://allseenalliance.org/developers/learn/core/standard-core)
* [Learn more about the AllJoyn Thin Core](https://allseenalliance.org/developers/learn/core/thin-core)
* [Learn more about the low-level details of the AllJoyn system](https://allseenalliance.org/developers/learn/core/system-description)
