#### AllJoyn bus 
[37IOT物联网开发社区](http://37iot.com)是国内专业的物联网开发技术论坛，欢迎各位有趣之士进入共同进步。

AllJoyn系统最基本的抽象就是AllJoyn总线。它为分布式系统提供了一个快速、轻量级的方式来传递消息序列。你可以将AllJoyn总线看作是消息传递的"高速公路"。图片显示了单一设备上AllJoyn总线实例在理论上的结构。
![prototypical-alljoyn-bus](https://allseenalliance.org/sites/default/files/developers/learn/standard-core/prototypical-alljoyn-bus.png)

Figure: Prototypical AllJoyn bus

典型的AllJoyn总线特性如下：
* 总线用加粗的水平黑线表示。垂直线可以被认为是消息通过总线在源点和目的点之间传递的"出口"。

* 所示的总线连接被描述为了六边形（这是任意选择的形状）。正如高速公路的出口通常都具有编号，图中每个连接都分配了唯一的连接名称。为了清晰起见，这里使用连接名称的简化形式。

* 许多情况下，总线上的连接都可以被认为是进程的合作方。因此，在上图的例子中，独特的连接名称:1.1可能被分配给应用程序实例进程的一个连接，而独特的连接名称:1.4可能被分配给其它应用程序实例进程的连接。AllJoyn总线的目标就是让两个应用程序进行通信，而无需处理底层机制的细节。其中一个连接可以认为是客户端存根，另一方就可以认为是服务器存根。

上图显示了AllJoyn总线的一个实例，并说明了软件总线如何给连接到总线上的组件提供进程间通信。AllJoyn总线的典型设备扩展如下图所示。组件可根据需要，在Smartphone和Linux主机上的组件之间创建逻辑总线段之间的通信链路。
![device-device-comm](https://allseenalliance.org/sites/default/files/developers/learn/standard-core/device-device-comm.png)

Figure: Device-to-device communication handled by the AllJoyn framework

通信链路的管理由AllJoyn系统负责，并且由许多底层技术组成，例如Wi-Fi和蓝牙技术。可能有不同的设备参与管理AllJoyn总线，但是这对分布式总线上的用户都是透明的。对于总线上的某个组成部分，分布式AllJoyn系统看起来就像是本地设备中的总线。

下图显示了分布式总线对于总线上的用户是如何呈现的。一个组件（例如，智能手机连接的名称为1.1）可以创建一个进程来调用Linux主机上的名称为1.7的组件，而无需担心该组件的物理位置。

![dist-bus-local-bus](https://allseenalliance.org/sites/default/files/developers/learn/standard-core/dist-bus-local-bus.png)

Figure: A distributed AllJoyn bus appears as a local bus

#### Bus router
上图说明了逻辑分布式总线实际上被分成了若干个段，每个段都运行在不同的设备上。AllJoyn从功能上实现这些逻辑总线段的进程被称为AllJoyn router。

Unix派生系统中的长驻守护进程，通常用来描述运行在计算机系统中并提供一些必要功能的程序。在Linux系统中，不叫做daemon，我们把它叫做单独的router(standalone router)。在Windows系统中，长驻服务更加常见，但是我仍然是指AllJoyn Router。
![bubble-diagram-bus]( https://allseenalliance.org/sites/default/files/developers/learn/standard-core/bubble-diagram-bus.png)

Figure: Relating bubble diagrams to the bus

为了让AllJoyn router更形象化，我们创建气泡图是很有用的。考虑两段AllJoyn总线，一段位于Smartphone上而另一段位于Linux主机，如上图所示。总线连接被标记为了客户端（C）和服务器（S）基于RMI模型。执行分布式总线核心的AllJoyn  router被标记为（D）。上图中的组件通常被解释为下图所示的插图。

![alljoyn-bubble-diagram](https://allseenalliance.org/sites/default/files/developers/learn/standard-core/alljoyn-bubble-diagram.png)

Figure: AllJoyn bubble diagrams

气泡可以被看作分布式系统上运行的计算机进程。左侧的两个客户端（C）和一个服务（S）进程运行在Smartphone上。这三个进程与Smartphone上的AllJoyn Router通信，实现了分布式AllJoyn总线上的本地网段。在右侧，也有一个Router，它在Linux主机上实现了AllJoyn总线的本地网段。这两个Router将协调整个逻辑总线的消息流，如图4所示，逻辑总线是单一的实体连接。类似Smartphone中的配置，Linux主机上有两个服务组件和一个客户端组件。

在此配置中，客户端组件C1可以采用远程方法来调用服务组件S1，就像它是一个本地对象。参数在源头进行封装，并由Smartphone上的Router送至本地总线段的路由。封装参数通过网络链接（从客户端来看是透明的）发送至Linux主机上的Router。而Linux主机上运行的Router确定目的地是S1，并且对封装参数进行拆封，然后通知服务去调用远程方法。如果有返回值，那么将反向进行这个通信过程，把返回值传回给客户端。

由于standalone Router在后台运行，并且客户端和服务都运行在单独的进程中，所以在每个单独进程中必须有一个Router的"代理"。 AllJoyn将调用这些代理总线附件。

#### Bus attachments
每个AllJoyn总线连接都需要一个特定的AllJoyn组件作为介质，它称为总线附件。每个需要连接AllJoyn总线的进程都有一个总线附件。

当在硬件和软件之间讨论软件组件时，往往会引出一个比喻。我们可以将分布式AllJoyn总线的本地网段想象为台式电脑的底板硬件总线。硬件总线本身就能传递电子信息，并且有一个可以插卡的点称为连接器。AllJoyn中类似功能的连接器就是总线附件。

AllJoyn总线附件是本地指定语言的对象，它代表了分布式AllJoyn总线中的客户端、服务或对等点。例如，这里有为用户提供总线附件功能的C++语言实现，还有为用户提供相同总线附件功能的Java语言实现。由于AllJoyn增加了语言支持，将会有更多这样的具体语言实现。

#### Bus methods bus properties and bus signals
AllJoyn基本上是一个面向对象的系统。在面向对象的系统中，人们将谈论对象中的调用方法（因此对于分布式系统，人们将谈论长驻远程方法调用）。面向对象编程中的对象需要有成员。通常，还需要对象方法或属性，在AllJoyn中就是BusMethods和BusProperties。AllJoyn同样也有总线信号（BusSignal），它是对象中某些事件或状态变化的异步通知。

为了使客户端、服务和对等点之间的通信更加透明，那么总线方法和总线信号中的参数顺序必须有一些规范，并且总线属性中也必须有一些形式的类型信息。对于计算机科学，方法（或信号）输入和输出类型的申明或定义被称为类型签名。

类型签名使用字符串来定义，包括所有的基本数字类型（对于大多数编程语言），以及从这些基本类型创建出的复合类型，例如数组和结构体。类型签名的具体分配和使用超出了本文介绍的范围，但是总线方法、信号或属性的类型签名将传递给AllJoyn底层系统，来实现总线上传递参数和封装返回值的转换。

#### Bus interfaces
在大多数面向对象的编程系统中，方法或属性集合将合并到具备固有内在联系的群组中。该集合函数的统一申明被称为接口。接口为执行接口规范的实体与外界之间的连接提供服务。正因为如此，接口需要适当的标准结构来实现标准化。各种网站上可以找到众多的接口服务规范，从电话到媒体播放器控制。D-Bus规范在XML中进行描述来指定接口。

接口规范会将总线方法、总线信号、总线属性以及与它们相关的类型签名组合到一个命名组中。而实际上，接口会由客户端、服务或对等点进程来实现。如果实现了给定的命名接口，那么在这个实现和外界之间会有一个隐性契约，接口将会支持它所有的总线方法、总线信号和总线属性。

接口名称通常采用域名反转形式。例如，这里有许多AllJoyn实现的标准接口。其中有一个标准接口是theorg.alljoyn.Businterface，将守护实现并为总线附件提供一些基本功能。

值得注意的是，接口名称仅仅是相对自由形式的命名空间中的一个字符串，并且其它命名空间可能也有类似的外观。接口名称字符串提供的特定函数不应该和其它类似的字符串混淆，特别是总线名称。例如，org.alljoyn.sample.chat可能是总线名称常量，就是客户端搜索的名字。但是也有可能org.alljoyn.sample.chat是接口名称，它定义了总线对象采用特定的总线名称连接到总线附件上时提供的方法、信号和属性。如果存在给定接口名称的接口就暗示了总线名称的存在；但是，它们确实是两个完全不同的东西，有时可能长得一模一样。

#### Bus objects and object paths
总线接口提供了一种标准的方式来声明分布式系统中的接口。总线对象提供桥接到可能实现给定接口规范的地方。总线对象存在于总线附件中，并作为通信的终点。

因为，在特殊的总线附件中，特定的接口可能会有多种实现，所以必须增加额外的结构体来区分这些接口实现。这将由对象路径来提供。

就像接口名称是命名空间中的一个字符串，对象路径同样也存在于命名空间中。命名空间的结构就像一棵树，路径的思维模式就是文件系统的目录树。事实上，对象路径的路径分隔符使用的是斜杠字符，和Unix文件系统类似。由于总线对象是总线接口的实现，所以对象路径可以遵循相应接口的命名约定。如果定义了一个磁盘控制器接口（exampleorg.freedesktop.DeviceKit.Disks），那么你就可以想象，对于同一系统中的两个分开的物理磁盘，这个接口的不同实现应该遵循下面的对象路径：
> /org/freedesktop/DeviceKit/Disks/sda1

> /org/freedesktop/DeviceKit/Disks/sda2 
