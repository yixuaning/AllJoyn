## Architecture
[37IOT物联网开发社区](http://37iot.com)是国内专业的物联网开发技术论坛，欢迎各位有趣之士进入共同进步。
### Network Architecture
AllJoyn框架运行在本地网络上。它可以让设备和应用去广告和发现对方。这一节讲述网络架构和各种各样的AllJoyn组件之间的关系。

#### Apps and Routers
AllJoyn框架包含AllJoyn应用和AllJoyn路由(Routers)，或者简称为应用和路由。应用与路由通信和路由与应用通信。应用和其他应用只能够通过路由来通讯。

应用和路由可以在同一个物理设备或者不同的设备上。从AllJoyn的立场来看，这不重要。事实上，有3种拓扑结构：

1. 一个应用使用他自己的路由。在这种情况下，路由被称作“Bundled Router”，因为它和应用绑定在了一起。手机操作系统(Android和iOS)和桌面操作系统(Mac OS X和Windows)上的AllJoyn应用通常使用这种结构。

2. 同一个设备上多个应用使用一个路由。在这种情况下。路由被称作“Standalone Router”并且它通常运行在一个后台/服务(backgroud/service)进程。在Linux系统中，普遍做法是AllJoyn路由运行在一个daemon进程中，其他的AllJoyn应用连接到独立的路由上。在同一个设备上让多个应用使用共同的Alljoyn路由，设备消耗更少的资源。

3. 一个应用使用一个在不同的设备上的路由。嵌入式设备(使用精简版(Thin variant)，稍后阐述)典型的使用这种方式，因为嵌入式设备没有足够的CPU和Memory去运行AllJoyn路由。
![apps-and-routers](https://allseenalliance.org/sites/default/files/developers/learn/apps-and-routers.png)

#### Transports
AllJoyn框架运行在本地网络上。目前它支持Wi-Fi、Ethernet、Serial和Power Line(PLC)，但是自从 AllJoyn software was written to be transport-agnostic并且 AllJoyn系统是一个进化的开源项目，将来可能会支持更多的传输。
此外，bridge software can be created to bridge the AllJoyn framework to other systems like Zigbee, Z-wave, or the cloud。事实上，一个工作团队正在为增加[网关代理( Gateway Agent)](https://wiki.allseenalliance.org/gateway/gatewayagent)而工作。 
### Software Architecture
AllJoyn网络包括AllJoyn应用和AllJoyn路由。
一个Alljoyn应用应该包含下列组件：
* [AllJoyn App Code](https://allseenalliance.org/developers/learn/architecture#alljoyn-app-code)
* [AllJoyn Service Frameworks Libraries](https://allseenalliance.org/developers/learn/architecture#alljoyn-service-frameworks-libraries)
* [AllJoyn Core Library](https://allseenalliance.org/developers/learn/architecture#alljoyn-core-library)

一个[AllJoyn路由](https://allseenalliance.org/developers/learn/architecture#alljoyn-router)可以单独运行也可以和AllJoyn Core Library绑定在一起。

![alljoyn-software-architecture](https://allseenalliance.org/sites/default/files/developers/learn/alljoyn-software-architecture.png)

#### AllJoyn Router
AllJoyn路由在AllJoyn Routers and Applications之间转发AllJoyn messages ，including between different transports.

#### AllJoyn Core Library
AllJoyn Core Library提供最底层的一套API去和AllJoyn Network交互。它提供了直接调用：
* 广告和发现
* 创建会话
* 接口定义(方法、属性，信号)
* 创建和处理对象

开发者使用这些API去实现AllJoyn服务框架或者私有的接口。
[Learn more about AllJoyn Core Frameworks. ](https://allseenalliance.org/developers/learn/core)

#### AllJoyn Service Framework Library
AllJoyn服务框架库实现了一套共同的服务，比如管理、通知或者控制面板。通过使用共同的AllJoyn服务框架，应用和设备可以正确的交互去执行特定的功能。
服务框架被分成了以下AllSeen工作组(AllSeen Working Groups):
* [Base Services](https://allseenalliance.org/developers/learn/base-services)

> [管理(Onboarding).](https://allseenalliance.org/developers/learn/base-services/onboarding)Provide a consistent way to bring a new device onto the Wi-Fi network.

> [配置(Configuration)](https://allseenalliance.org/developers/learn/base-services/configuration). Allows one to configure certain attributes of an application/device, such as its friendly name.

> [通知(Notifications)](https://allseenalliance.org/developers/learn/base-services/notification). Allows text-based notifications to be sent and received by devices on the AllJoyn network. Also supports audio and images via URLs.

> [控制面板(Control Panel)](https://allseenalliance.org/developers/learn/base-services/controlpanel). Allows devices to advertise a virtual control panel to be controlled remotely.

* [更多的服务框架(More Service Frameworks)](https://wiki.allseenalliance.org/). More service frameworks are actively being developed by the AllSeen Working Groups.

鼓励开发人员尽可能的使用AllJoyn服务框架(AllJoyn Service Framework)。如果一个存在的服务不可用，开发人员可以和AllSeen Alliance共同开发一个标准的服务。在某些情况下，使用私有的服务和接口更合情合理；然而，这些服务不能交互，没法充分利用设备和应用的Alljoyn生态系统。

#### AllJoyn App Code
这是AllJoyn应用的应用逻辑。无论是提供高等级功能的Alljoyn服务框架库(AllJoyn Service Frameworks Libraries)或者提供直接调用的AllJoyn核心API(AllJoyn Core APIs)的Alljoyn核心库(AllJoyn Core Library)都是可编程的。

#### Thin and Standard
AllJoyn framework提供了两个版本：
* 标准的(Standard).为非嵌入式设备，比如Android,iOS,Linux。
* 精简的(Thin).为资源有限的嵌入式设备，比如Arduino,ThreadX,低内存Linux。

![alljoyn-standard-and-thin](https://allseenalliance.org/sites/default/files/developers/learn/alljoyn-standard-and-thin.png)
### Programming Models
#### AllSeen Core Framework
典型的，编写应用会通过使用AllJoyn服务框架API(AllJoyn Service Framework APIs )以便应用能够和使用相同服务框架的设备兼容。只有采用AllJoyn服务框架的AllSeen工作组开发的应用程序会在AllSeen生态系统与其他应用程序和设备兼容。
如果一个应用程序想实现它自己的服务，它可以通过对AllJoyn核心API(AllJoyn Core APIs)直接编程。当你这样做的时候，建议遵循事件(Events)和行为(Actions)公约去在其他的AllJoyn设备间实现特定的交互。
应用可以同时使用服务框架(Service Framework)和核心API(Core APIs)。

#### Data-Driven API
AllJoyn的数据驱动的API(DDAPI)是一个AllJoyn核心框架(AllJoyn Core framework)的替代API。它是建立在AllJoyn核心API的顶部，是专门为物联网特别定制的用例。取代核心框架的服务导向(service-oriented)的范例，它使用发布/订阅(publish/subcribe)模式。

For more Information, go [here](https://allseenalliance.org/developers/learn/ddapi).

[Learn more about Events and Actions.](https://allseenalliance.org/developers/learn/core/events-and-actions)
