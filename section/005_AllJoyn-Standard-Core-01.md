## AllJoyn™ Standard Core   
[37IOT物联网开发社区](http://37iot.com)是国内专业的物联网开发技术论坛，欢迎各位有趣之士进入共同进步。
### Overview 
AllJoyn框架是一个开源的软件系统，它为分布式应用运行在不同的设备提供了环境，注重移动性，安全性和动态配置。AllJoyn系统解决了异构分布式系统内在的固有问题 and addresses the unique issues that arise when mobility enters the equation。这让应用开发者可以专注于核心应用程序的构建。 

AllJoyn框架是“平台无关”的(platform-neutral)，它被设计成尽可能独立的规范对于操作系统、硬件和设备运行的软件。事实上，AllJoyn框架可以运行在微软Windows，Linux，Android，iOS，OS X，OpenWrt，和作为Unity游戏开发生态系统中的一个Unity插件。 

AllJoyn框架设计的理念是近距离和移动性。在移动环境中，设备将不断地进入和离开其他设备，底层网络性能也会改变。 

The AllJoyn SDKs are available at (http://www.allseenalliance.org).

使用AllJoyn框架的应用程序的类型十分广泛，只受开发者的想象力限制。扩展社交网络就是一个例子。用户可以定义一个兴趣爱好的配置。一旦进入一个地方，AllJoyn时能的手机会立刻发现其他在附近的有相同兴趣的同事，创建对等设备之间的通信网络，让他们进行沟通，交流信息。 

今天，大多数的手机集成了Wi-Fi，所以如果两个用户走进一个有Wi-Fi热点的家或办公室，他们的设备可以连接到接入点，利用额外的网络容量。此外，他们的设备可以定位在附近的其他设备（通过Wi-Fi覆盖区域定义），可以发现在其他设备的额外服务，并使用这些服务，如果需要的话。此外，它可以利用一个混合的拓扑连接，来让设备利用AllJoyn瘦库(Thin Library)指定使用蓝牙传输。因此，一旦连接到一个运行AllJoyn框架的设备，该设备可以和应用在Wi-Fi下交互。 

实时多玩家游戏是AllJoyn框架应用的另外一个实例。例如，一个多人游戏可以使用不同的设备如笔记本电脑，平板电脑和手机，和不同的底层网络技术如Wi-Fi。基础设施管理的细节都有AllJoyn框架处理，让游戏作者集中在游戏的设计与实现，而不是处理的点对点网络的复杂性。 

AllJoyn生态系统扩展，可以想象到各样的应用程序，例如： 
* Create a playlist consisting of music, and stream the songs to an AllJoyn-enabled car stereo system, or store them on a home stereo (subject to digital rights management)
* Sync recent photos or other media to an AllJoyn-enabled digital picture frame or television upon returning home from an event or trip
* Control home appliances such as televisions, DVRs, or game consoles
* Interact and share content with laptops and desktop computers in the area
* Engage in project collaboration between colleagues and students in enterprise and educational settings
* Provide proximity-based services like distributing coupons or vcards

### Benefits of the AllJoyn Framework 
As mentioned, the AllJoyn framework is a platform-neutral system that is designed to simplify proximity networking across heterogeneous distributed mobile systems. 

Heterogeneous in this case means not only different devices, but different kinds of devices (e.g., PCs, handsets, tablets, consumer electronics devices) running on different operating systems, using different communication technologies. 

**Open source**

The AllJoyn framework is being developed as an open source project. This means that all of the AllJoyn codebase is available for inspection, and developers are encouraged to contribute additions and enhancements. If the AllJoyn framework is missing a feature, you can contribute. If you run into a snag using the AllJoyn framework, or have a technical question, other participants in the open source community are ready and willing to provide help and guidance. The AllJoyn codebase is available at (http://www.allseenalliance.org). 

**Operating system independence**

The AllJoyn framework provides an abstraction layer allowing AllJoyn framework code and its applications to run on multiple OS platforms. As of this writing, the AllJoyn framework supports most standard Linux distributions including Ubuntu, and runs on Android 2.3 (Gingerbread) and later smartphones and tablets. The AllJoyn framework code also runs and is tested and validated on commonly available versions of the Microsoft Windows operating system including Windows XP, Windows 7, Windows RT, and Windows 8. Additionally, the AllJoyn framework code runs on Apple operating systems iOS and OS X, on embedded operating systems such as OpenWRT, and works with the Unity game development ecosystem. 

**Language independence**

Currently, developers may create applications using C++,Java, C#, JavaScript, and Objective-C. 

**Physical network and protocol independence**

There are many technologies available to networked devices. The AllJoyn framework provides an abstraction layer that defines clean interfaces to the underlying network stacks and makes it relatively easy for a competent software engineer to add new networking implementations. 

For example, as of this writing, the Wi-Fi Alliance has recently released a specification for Wi-Fi Direct, which will allow for point-to-point Wi-Fi connectivity. A networking module for Wi-Fi Direct is actively being developed that will transparently add Wi-Fi Direct and its pre-association discovery mechanisms to the available networking options for AllJoyn developers. 

**Dynamic configuration**

Often, as a mobile device makes its way through the various locations it encounters during its lifetime, associations with networks may come and go. This means that IP (Internet Protocol) addresses may change, network interfaces may become unusable, and services may be transitory. 

The AllJoyn framework notices when old services are lost and new services appear, and forms new associations when required. The AllJoyn framework is primed and ready as an application layer for Wi-Fi Hotspot 2.0 - a technology that aims to bring the roaming transparency of cell phones and cell towers to Wi-Fi hotspots. 

**Service advertisement and discovery**

Whenever devices need to communicate, there must be some form of service advertisement and discovery. In the old days of static networks, human administrators made explicit arrangements to enable devices to communicate. More recently, the concepts of zero configuration networks have been popularized, especially with Apple Bonjour, and Microsoft Universal Plug and Play. We also see existing technology-specific discovery mechanisms available such the Bluetooth Service Discovery Protocol and emerging mechanisms such as the Wi-Fi Direct P2P Discovery specification. The AllJoyn framework provides a service advertisement and discovery abstraction that simplifies the process of locating and consuming services. 

**Security**

The natural model for security in distributed applications is application-to-application. Unfortunately, in many cases, the network security model does not match this natural arrangement. For example, the Bluetooth protocol requires pairing between devices. Using this approach, once devices are paired, all applications on both devices are authorized. This may not be desirable when considering something more capable than a Bluetooth headset. For example, if two laptops are connected over Bluetooth, a much finer granularity is necessary. The AllJoyn framework is designed to provide extensive support for complex security models such as this, with an emphasis on application-to-application communication. 

Object model and remote method invocation 
The AllJoyn framework utilizes an easy-to-understand object model and Remote Method Invocation (RMI) mechanism. The AllJoyn model re-implements the wire protocol set forth by the D-Bus specification and extends the D-Bus wire protocol to support distributed devices. 

Software componentry 
Along with a standard object model and wire protocol comes the ability to standardize various interfaces into components. In much the same way that a Java Interface declaration provides a specification to interact with a local instantiation of an implementation, the AllJoyn object model provides a language-independent specification to interact with a remote implementation. 

Using a specification, many interface implementations can be considered, thereby enabling standard definitions for application communication. This is the enabling technology for software componentry. Software components are at the heart of many modern systems, and are especially visible in systems such as Android, which defines four primary component types as the only way to participate in the Android Application Framework; or in Microsoft systems which use descendants of the Component Object Model (COM) system. 

We expect that a rich "sea" of interface definitions will emerge in order to enable the scenarios described in Overview. The AllJoyn project expects to work with users to define and publish standard interfaces and support the sharing of implementations. 

### Conceptual Overview  
AllJoyn框架包含许多帮助理解和叙述不同的pieces的抽象。只需要知道少数关键的抽象来理解AllJoyn-Based Systems。

本节提供了一个AllJoyn框架的high-level view,为后续的文档，如详细的API文档提供了基础。 

Remote Method Invocation 
Distributed systems are groups of autonomous computers communicating over some form of network in order to achieve a common goal. Consider the ability of a program running in one address space on one machine to call a procedure located in another address space on a physically separate machine as if it were local. This is usually accomplished through Remote Procedure Call (RPC) or, if object-oriented concepts are in play, RMI or Remote Invocation (RI).

The basic model in an RPC exchange involves a client, which is the caller of the RPC, and a server (called a service in the AllJoyn model), which actually executes the desired remote procedure. The caller executes a client stub that looks just like a local procedure on the local system. The client stub packages up the parameters of its procedure (called marshaling or serializing the parameters) into some form of message and then calls in to the RPC system to arrange delivery of the message over some standard transport mechanism such as the Transmission Control Protocol (TCP). At the remote machine, there is a corresponding RPC system running, which unmarshals (deserializes) the parameters and delivers the message to a server stub that arranges to execute the desired procedure. If the called procedure needs to return any information, a similar process is used to convey the return values back to the client stub, which in turn returns them to the original caller.

Note that it is not required that a given process only implement a client personality or a service personality. If two or more processes implement the same client and service aspects,
they are considered peers. In many cases, AllJoyn applications will implement similar functionality and be considered peers. The AllJoyn framework supports both classic client and service functions and also peer-to-peer networking.















