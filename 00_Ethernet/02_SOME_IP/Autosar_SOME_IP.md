# 1 &#8194;SOME/IP 概述

## 1.1 &#8194;背景及推动因素

&#8194;&#8195;传统的车载通信协议（e.g. CAN），由于可扩展性、带宽和灵活性等方面的限制，难以适应汽车智能化和网联化的发展，相比之下，基于以太网的通信方案能够满足不断增长的通信需求，以支持车内各组件间以及其与外部服务之间的互联互通。

&#8194;&#8195;近几十年来，以太网已被证实是通信系统中一种灵活、可扩展的网络技术。以太网的主要优势之一是支持多种物理介质，这使其可以应用于汽车。由于物理介质与协议无关，因此可以轻松开发或调整其他传输技术，以满足汽车行业要求。

&#8194;&#8195;各个 ECU 需要互相通信以实现各种复杂功能，例如引擎控制、刹车系统等（这类传统的信息交互内容传输相对简单）；随着汽车对于外部服务的需求增加，以自动驾驶和智能座舱为代表，如导航、音视频娱乐系统等，车内通信的复杂程度进一步升级。

&#8194;&#8195;基于 CAN（FD）总线的应用通信（非诊断通信）采用了面向信号的方式，这是一种根据发送方需求实现的通信过程，当发送方发现信号值变化或达到预定的发送周期，即会向外进行数据传输，而不考虑接收方需求。

&#8194;&#8195;以引擎和制动系统联动为例，在基于 CAN 的通信中，引擎 ECU 通过周期性地向制动 ECU 发送引擎转速信号（RPM），以确保制动系统能够根据引擎状态进行调整。此时，通信是以数据信号（RPM 值）为核心，以实时性和可靠性为重点。

&#8194;&#8195;而基于 SOME/IP 的通信则更面向服务，本质上，是在（服务）接收方有需求时才进行交互。汽车中的不同 ECU 可以使用SOME/IP 来请求和提供以下服务，如获取导航路线、播放音乐、执行自动驾驶功能等。

&#8194;&#8195;这种方式使得总线上不会出现过多不必要的数据收发，从而有效降低了总线负载。

&#8194;&#8195;需要注意的是，SOME/IP 协议不仅仅限于描述通信，确切的说，SOME/IP 是一种对 ECU 软件组件产生影响的中间件。因此，AUTOSAR 中有一个单独的软件路径，可连接到应用程序。

## 1.2 &#8194;起源及协议

&#8194;&#8195;2011 年，BWM 设计 SOME/IP 协议，随后通过 AUTOSAR 规范发布公开，并广泛应用于车载以太网。

&#8194;&#8195;本文参考协议包括以下：

&#8194;&#8194;&#8195;- AUTOSAR_SOME/IP Protocol Specification

&#8194;&#8195;&#8195;揭示了 SOME/IP 协议的两个重要内容 —— **序列化 / Serialization**和**远程服务调用 / Remote Procedure Call (RPC)**

&#8195;&#8195;&#8195;- 序列化指将数据结构或对象按定义规则转换成二进制序列的过程，定义了数据格式规范。

&#8195;&#8195;&#8195;- RPC，通过在网络上进行数据传输，以实现一方到另一方的服务调用。
 
&#8194;&#8194;&#8195;- AUTOSAR_SOME/IP Service Discovery Protocol Specification

&#8194;&#8195;&#8195;揭示了 SOME/IP SD 协议的两个主要内容 —— **服务发现 / Service Discovery**和**服务发布/订阅 / Service Pulish/Subscribe**

&#8195;&#8195;&#8195;- 基于 SD，客户端可寻找自身所需的服务，客户端也可表明能够提供哪些服务；以此，客户端与服务端可动态建立连接。

&#8195;&#8195;&#8195;- Publish/Subscribe，客户端可以向服务端订阅所需的数据，服务端以周期或其他触发方式发布该数据。

## 1.3 &#8194;定义及概念

### 1.3.1 &#8194;SOME/IP

&#8194;&#8195;全称 Scalable service- Oriented MiddlewarE over IP

>&#8194;&#8195;SOME/IP is an automotive/embedded communication protocol which supports remote procedure calls, event notifications and the underlying serialization/wire format.

>&#8194;&#8195;SOME/IP shall be implemented on different operating system (i.e. AUTOSAR, GENIVI, and OSEK) and even embedded devices without operating system. SOME/IP shall be used for inter-ECU Client/Server Serialization. An implementation of SOME/IP allows AUTOSAR to parse the RPC PDUs and transport the signals to the application.

&#8194;&#8195;**Scalable / 可扩展**
    
&#8194;&#8194;&#8195;- 适配不同的车载设备。
    
&#8194;&#8194;&#8195;- 适配不同的操作系统：AUTOSAR / OSEK / Linux。
    
&#8194;&#8194;&#8195;- 动态服务发布与订阅。
    
&#8194;&#8195;Note：借助 SOME/IP 协议的高度平台扩展性，可实现不同平台的数据交互，因此统一的 SOME/IP 通信机制是不同平台相互通信的前提条件。

&#8194;&#8195;**service-Oriented / 面向服务**

&#8194;&#8194;&#8195;- 解决服务提供者和消费者之间的网络通信问题。
 
&#8194;&#8194;&#8195;- SOME/IP 报文定义了服务接口的内容。

&#8194;&#8195;**MiddlewarE / 中间件**

&#8194;&#8194;&#8195;- 处于独立操作系统 / 硬件和应用层之间的系统软件或服务程序。

&#8194;&#8194;&#8195;- 独立于硬件平台、操作系统和编程语言，不受底层硬件和操作系统复杂性的影响。

&#8194;&#8194;&#8195;- 与应用层之间使用标准的API连接。

&#8194;&#8195;**over IP**

&#8194;&#8194;&#8195;- 基于 TCP/IP 协议，使用以太网传输。

### 1.3.2 &#8194; SOA

&#8194;&#8195;Service-Oriented Architecture / 面向服务的架构，简称 SOA，是一种软件设计理念 / 软件架构模式。

&#8194;&#8195;它将应用程序的不同功能单元（服务）通过定义良好的接口和协议进行结合；这些功能（服务）是独立且可复用的，其横跨多个系统和组织进行交互。

&#8194;&#8195;SOA 的目的在于提高软件系统的灵活性、可扩展性以及可维护性。

&#8194;&#8195;SOA 特性包括以下：

&#8194;&#8194;&#8195;- **松耦合，可重用的服务**

&#8194;&#8195;&#8195;所谓松耦合，即服务与服务间不相互影响，某一个服务发生变更，其他服务不应收到干扰；可重用的目的在于面向更多的服务对象。

&#8194;&#8194;&#8195;- **通过精确定义的服务接口进行通信**

&#8194;&#8195;&#8195;为实现上一条原则，就需要稳定的服务接口；这也是 SOA 架构能为汽车行业带来增益的重要原因。

&#8194;&#8195;SOA 组成包括：服务提供方、服务消费方、服务注册中心；

&#8194;&#8195;SOA 基本操作包括：服务发布、服务订阅、服务绑定与调用。

&#8194;&#8195;**SOA 和 SOME/IP 的联系**

&#8194;&#8194;&#8195;SOA 设计的是“服务”架构，包含了服务（应用程序）和可调用接口；SOME/IP 是协议，打包了“服务接口”，也就是应用程序的对外接口。

&#8194;&#8194;&#8195;即，SOA 设计内容中的接口部分，会通过 SOME/IP 传输实现，可以将 SOME/IP 视为 SOA 的中间件。

&#8194;&#8194;&#8195;注：由于 SOME/IP 本身也是一种面向服务的协议，所以一般认为 SOME/IP 只不过是一种实现 SOA 可行的协议选择；即，可以通过 SOME/IP 用来实现 SOA，但 SOA 的实现却不一定非得用 SOME/IP。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-SOA_Structure1.png width="540px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-SOA_Structure2.png width="540px">

### 1.3.3 &#8194; 术语

&#8194;&#8195;**Service / 服务**

&#8194;&#8194;&#8195;- 提供特定功能或任务的软件组件或应用程序 / 实现某种功能的函数或方法。

&#8194;&#8194;&#8195;- 离散的功能单元，可以被远程访问并独立执行和更新。

&#8194;&#8195;**Service Interface / 服务接口**

&#8194;&#8194;&#8195;- 所谓接口，即 API，全称为应用程序编程接口，软件组件之间信息交互的桥梁，即通过 API，不同的软件系统组件能够相互“对话”，而无需不断构建新的连接基础架构。
    
&#8194;&#8194;&#8195;- 每个服务（器）都会提供多种不同的接口。比如，麦当劳点餐服务，会提供查看菜单、下单、订单查询等接口。
    
&#8194;&#8194;&#8195;- 从软件开发角度，接口定义就是对如何请求信息，以及如何响应信息的结构规定。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Autosar_Structure.png width="450px">

# 2 &#8194;SOME/IP Protocol Specification

## 2.1 &#8194;Specification of SOME/IP Protocol - Remote Procedure Call / RPC / 远程服务调用

&#8194;&#8195;RPC，允许 Client 端通过网络（与总线类型并无关系）请求调用 Server 端的服务（应用程序），但不需要了解如何实现；达到“用别人的东西，仿佛是自己的一样”这种效果。

&#8194;&#8195;RPC，揭示了 SOME/IP 协议的运行逻辑。

&#8194;&#8195;SOME/IP 协议对于**服务方式**的具体描述包含如下内容：

>&#8194;&#8195;SOME/IP provides service oriented communication over a network. It is based on service definitions that list the functionality that the service provides.
>
>&#8194;&#8195;A service can consist of combinations of zero or multiple events, methods and fields. 
>
>&#8194;&#8195;**Events provide data that are sent cyclically or on change from the provider to the sub-scriber.**
>
>&#8194;&#8195;**Methods provide the possibility to the subscriber to issue remote procedure calls which are executed on provider side.**
>
>&#8194;&#8195;**Fields are combinations of one or more of the following three:**
>
>&#8194;&#8194;&#8195;a notifier which sends data on change from the provider to the subscribers.
>
>&#8194;&#8194;&#8195;a getter which can be called by the subscriber to explicitly query the provider for the value.
>
>&#8194;&#8194;&#8195;a setter which can be called by the subscriber when it wants to change the value on provider side.

&#8194;&#8195;SOME/IP 协议同样定义了三种**通信机制**：

&#8194;&#8194;&#8195;- Request/Response Communication：最常见的通信方式之一，Client 发送请求，Server 给予响应。

&#8194;&#8194;&#8195;- Fire&Forget Communication：Client 发送请求，但无需响应。

&#8194;&#8194;&#8195;- Notification Events：Notification 表明的是发布/订阅的概念，通常，Server 端发布服务，Client 订阅该服务。

&#8194;&#8195;**Strategy for sending notifications**

&#8194;&#8194;&#8195;针对不同用例，常见的策略包括以下：

&#8194;&#8194;&#8195;- 周期性更新 / Cyclic update：按固定周期更新数据，e.g. every 100 ms for safety relevant messages with Alive.

&#8194;&#8194;&#8195;- 变化性更新 / Update on change：只要数值发生改变就会更新数据，e.g. door open.

&#8194;&#8194;&#8195;- 超限性更新 / Epsilon change：当前数据与上一轮数据的差值大于某个设定值时，进行更新。"This concept may be adaptive, i.e. the prediction is based on a history; thus, only when the difference between prediction and current value is greater than epsilon an update is transmitted."

>&#8194;&#8194;SOME/IP is used only for transporting the updated value and not for the publishing and subscription mechanisms. These mechanisms are implemented by SOME/IP-SD.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Service_Communication.png width="400px">

&#8194;&#8195;根据以上，不同服务方式采用的通信机制存在差异：

&#8194;&#8195;**Events**，采用 Notification 机制

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Event_communication.png width="360px">

&#8194;&#8195;事件组至少包含一个事件。

>&#8194;&#8195;Eventgroup is a logical grouping of events and notification events of fields inside a service in order to allow subscription.
>
>&#8194;&#8195;Events as well as field notifiers shall be mapped to at least one SOME/IP Eventgroup.

&#8194;&#8195;**Method**，涉及 Request/Response 和 Fire&Forget 机制

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Method_communication.png width="360px">

&#8194;&#8195;**Field**中，Setter 和 Getter 对应是所谓**配置和获取服务**，使用 Request/Response 机制，而 Notifier 则采用了 Notification机制

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Field_communication.png width="600px">

&#8194;&#8195;**Method vs Field vs Event 解析**

&#8194;&#8194;&#8195;Method，强调的是功能的调用，例如控制空调的打开或者关闭；

&#8194;&#8194;&#8195;Event，本质就是事件，其特点在于可以不涉及任何参数，例如空调出现某个故障或切换到某个模式；

&#8194;&#8194;&#8195;Field 与前面两种方式最大的区别在于，其核心是对具体数据的操控，一般来说，这个数据是一个具有实际意义的变量；相比之下，Method 和 Event 主要目的在于执行某些动作，虽然这些动作可能会涉及到对于变量的操控，但在设计时，将变量操作设计为 Field 类型，或许更合理。例如，设定/获取空调当前的温度值。

&#8194;&#8194;&#8195;此外，Notifier 在订阅之后应立即返回一个初始值，Event 不涉及。

>&#8194;&#8194;&#8195;A field represents a status and has a valid value. The consumers subscribing for the field instantly after subscription get the field value as an initial event.
>
>&#8194;&#8194;&#8195;The major difference between the notifier of a field and an event is that events are only sent on change, the notifier of a field additionally sends the data directly after subscription.

## 2.2 &#8194;Specification of SOME/IP Message Format (Serialization / 序列化)

&#8194;&#8195;序列化是指将数据结构或对象按定义的规则转换成二进制串的过程。

&#8194;&#8195;反序列化是指将二进制串依据相同规则重新构建成数据结构或对象的过程。

&#8194;&#8195;SOME/IP 通过序列化，使数据按照固定格式进行编排成为字节序，实现数据在网络上的高效传输。

&#8194;&#8195;**其本质就是定义数据在使用 SOME/IP 协议进行传输时，所符合的统一格式规范。**

>&#8194;&#8195;所谓序列化，在 AUTOSAR 中是指数据在 PDU 中的表达形式，可以理解为来自应用层的真实数据转换成固定格式的字节序，以实现数据在网络上的传输。
>
>&#8194;&#8195;软件组件将数据从应用层传递到RTE层，在 RTE 层调用 SOME/IP Transformer，执行可配置的数据序列化（Serialize）或反序列化（Deserialize）。
>
>&#8194;&#8195;SOME/IP Serializer 将结构体形式的数据序列化为线性结构的数据；SOME/IP Deserializer 将线性结构数据再反序列化为结构体形式数据。
>
>&#8194;&#8195;在服务端，数据经过 SOME/IP Serializer 序列化后，被传输到服务层的 COM 模块；在客户端，数据从 COM 模块传递到 SOME/IP Deserializer 反序列化后再进入 RTE 层。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Autosar_COM.png width="600px">

### 2.2.1 &#8194;Limitation

>&#8194;&#8195;Reordering of out-of-order segments of a SOME/IP message is not supported.

### 2.2.2 &#8194;Endianess

>&#8194;&#8195;All SOME/IP Header Fields shall be encoded in network byte order (big endian).
>
>&#8194;&#8195;The byte order of the parameters inside the payload shall bedefined by configuration.

&#8194;&#8195;大端（Big Endian）： 数据的高位字节存放在低地址，低位字节存放在高地址。

&#8194;&#8195;小端（Little Endian）： 数据的低位字节存放在低地址，高位字节存放在高地址。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Endianess.png width="600px">

### 2.2.3 &#8194;SOME/IP Header

**SOME/IP Header Format**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Header.png width="640px">

**SOME/IP Header and E2E header Format**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-E2E_Header.png width="640px">

#### 2.2.3.1 &#8194;Message ID / 32 Bit

>&#8194;&#8195;The assignment of the Message ID is up to the user / system designer. However, the Message ID is assumed be unique for the whole system (i.e. the vehicle).
>
>&#8194;&#8195;The Message ID shall be a 32 Bit identifier that is used to identify:
>
>&#8194;&#8194;&#8195;1. the RPC call to a method of an application
>
>&#8194;&#8194;&#8195;2. or to identify an event.

&#8194;&#8195;Message ID (32 bit) = Service ID (16 bit) + Method ID (16 bit)

&#8194;&#8195;Message ID 本质：区分服务，且每个服务仅定义唯一一个 Service ID。

&#8194;&#8195;通常建议，Method ID 最高位为 0 表示 "Method"（包括服务方式中定义的 Method、Field.Getter 以及 Field.Setter），最高位为1设置为 "Event"（包括服务方式中定义的 Event 以及 Field.Notifier）。

&#8194;&#8195;e.g. 请求媒体系统播放下一首音乐，Service ID 表示的就是音乐播放器Format，Method ID 指明切换至下一首这个操作。

>&#8194;&#8195;It is common practise and recommended to split the ID space of the Method ID between Methods and Events/Notifications. Methods would be in the range 0x0000-0x7FFF (first bit of Method-ID is 0 and Events/Notifications would use the range 0x8000-0x8FFF (first bit of the Method-ID is 1).

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Header_Message_ID.png width="400px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Special_Service_ID.png width="480px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Special_Method_ID.png width="480px">

&#8194;&#8195;Magic Cookie 是一个只有报头无负载的 SOME/IP 报文，用于确认 TCP 是否仍在连接中

&#8194;&#8195;Client 发给 Server 与 Server 发给 Client 的 Magic Cookie 是各种独立的，即不需要接收到对方发送的 Magic Cookie 才能发送己端的。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Magic_Cookie_Message.png width="640px">

#### 2.2.3.2 &#8194;Length / 32 Bit

&#8194;&#8195;其涵盖的范围是 Request ID 开始至 SOME/IP 报文结束。

#### 2.2.3.3 &#8194;Request ID / 32 Bit

>&#8194;&#8195;The Request ID allows a server and client to differentiate multiple parallel uses of the same method, getter or setter.

&#8194;&#8195;**响应规则**

>&#8194;&#8195;When generating a response message, the provider shall copy the Request ID from the request to the response message.

&#8194;&#8195;**复用规则**

>&#8194;&#8195;Request IDs must not be reused until the response has arrived or is not expected to arrive anymore (timeout).

&#8194;&#8195;Request ID 解析：

&#8194;&#8194;&#8195;- 对于 Method、Field.Getter 以及 Field.Setter（常见R&R机制），Server 和 Client 可通过 Request ID 来区分以上服务方式的多个并行使用，即，唯一确定一条请求。

&#8194;&#8195;&#8195;e.g. 前后两次控制娱乐系统切换音乐时（播放下一首），两次的 Request ID 并不相同，以此区分前后两次请求。

&#8194;&#8194;&#8195;- 对于 Event 以及 Field.Notifier，Request ID 可用于区分同一 Notification 的多次通知。

&#8194;&#8194;&#8195;- 对于 SOME/IP TP 报文，Request ID 可用于识别各分段属于同一源消息。

>&#8194;&#8195;In AUTOSAR the Request ID shall be constructed of the Client ID and Session ID.

&#8194;&#8195;Request ID (32 bit) = Client ID (16 bit) + Session ID (16 bit)

&#8194;&#8195;**Client ID**

&#8194;&#8194;&#8195;- Client ID 本质：区分同一服务的不同请求客户端，e.g. 汽车方向盘控件与副驾控制面板均可操作娱乐系统音乐播放器切换至下一首歌曲，那么就可以通过不同的Client ID来区分。

&#8194;&#8194;&#8195;- 对于 R&R，F&F 类通信机制的调用，需通过 Client ID 以区分客户端；**对于 Notification 类通信机制的调用，Client ID 应设置为 0x0000。**

&#8194;&#8194;&#8195;- Client ID 可通过支持添加固定前缀或固定数值等方式在全域内唯一。

&#8194;&#8195;**Session ID**

&#8194;&#8194;&#8195;- Session ID 本质：区分同一客户端请求同一服务的次数 / 同一 Event 的多次发送 / TP 属于同一源报报文。

&#8194;&#8194;&#8195;- 要求 R&R 通信机制必须使用；SOME/IP-SD 必须使用；SOME/IP-TP 必须使用。

&#8194;&#8194;&#8195;- Session ID 取值范围为 **0x0000 ~ 0xFFFF**

&#8194;&#8195;&#8195;1. 当 Session Handling 未激活时，Session ID = 0x0000；

>&#8194;&#8195;&#8195;For notification messages, a receiver shall ignore the Session ID in case Session Handling is not active.

&#8194;&#8195;&#8195;2. 当 Session Handling 激活时，Session ID 应随着请求次数在 0x0001 ~ 0xFFFF 内递增循环，满 0xFFFF 则从 0x0001 开始重新计数。

>&#8194;&#8195;&#8195;Request/Response methods shall use session handling with Session IDs.

&#8194;&#8194;&#8195;- 对于 R&R，若服务请求与响应的 Session ID 无法匹配，服务请求方应忽略该响应。

>&#8194;&#8195;The Client ID is the unique identifier for the calling client in-side the ECU. The Client ID allows an ECU to differentiate calls from multiple clients to the same method.
>
>&#8194;&#8195;The Session ID is a unique identifier that allows to distinguish sequential messages or requests originating from the same sender from each other.
>
>&#8194;&#8195;The Client ID shall also support being unique in the overall vehicle by having a configurable prefix or fixed value (e.g. the most significant byte of Client ID being the diagnostics address or a configured Client ID for a given application/SW-C).

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Header_Request_ID.png width="400px">

#### 2.2.3.4 &#8194;Protocol Version / 8 Bit

&#8194;&#8195;Protocol Version 应设置为 0x01。

>&#8194;&#8195;The Protocol Version shall be increased, for all incompatible changes in the SOME/IP header. A change is incompatible if a receiver that is based on an older Protocol Version would not discard the message and process it incorrectly.
>
>&#8194;&#8195;The Protocol Version shall not be increased for changes that only affect the Payload format.

#### 2.2.3.5 &#8194;Interface Version / 8 Bit

&#8194;&#8195;当前正使用的服务接口主版本；当服务接口发现不兼容变更时，需升级版本号；检测服务定义的一致性。

>&#8194;&#8195;Interface Version shall be an 8 Bit field that contains the Major Version of the Service Interface.

#### 2.2.3.6 &#8194;Message Type / 8 Bit

&#8194;&#8195;Message Type 本质：区分报文的不同类型。

&#8194;&#8195;当没有错误发生时，常规请求（Message Type = 0x00）应由响应（Message Type = 0x80）回复；若出现错误，则应响应 Error（Message Type = 0x81）。

>&#8194;&#8195;The 3rd highest bit of the Message Type (=0x20) shall be called TP-Flag and shall be set to 1 to signal that the current SOME/IP message is a segment.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Header_Message_Type.png width="600px">

#### 2.2.3.7 &#8194;Return Code / 8 Bit

&#8194;&#8195;Return Code 本质：在响应报文中反馈请求是否被正确执行。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Header_Return_Code_1.png width="600px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Header_Return_Code_2.png width="600px"> 

#### 2.2.3.8 &#8194;Payload / variable size

&#8194;&#8195;Payload 即数据负载（might consists of data elements for events or parameters for methods），该部分涉及数据序列化。

>&#8194;&#8195;The size of the SOME/IP payload field depends on the transport protocol used. With UDP the SOME/IP payload shall be between 0 and 1400 Bytes. The limitation to 1400 Bytes is needed in order to allow for future changes to protocol stack (e.g. changing to IPv6 or adding security means). Since TCP supports segmentation of payloads, larger sizes are automatically supported.

### 2.2.4 &#8194;Serialization of Data Structures

>&#8194;&#8195;The serialization is based on the parameter list defined by the interface specification.
>
>&#8194;&#8195;The interface specification defines the exact position of all data structures in the PDU and has to consider the memory alignment.
>
>&#8194;&#8195;Alignment is used to align the beginning of data by inserting padding elements after the data in order to ensure that the aligned data starts at certain memory addresses.
>
>&#8194;&#8195;There are processor architectures which can access data more efficiently (i.e. master) when they start at addresses which are multiples of a certain number (e.g multiples of 32 Bit).
>
>&#8194;&#8195;Alignment of data shall be realized by inserting padding elements after the variable size data if the variable size data is not the last element in the serialized data stream.
>
>&#8194;&#8195;There shall be no padding behind fixed length data elements to ensure alignment of the following data. (If data behind fixed length data elements shall be padded, this has to be explicitly considered in the data type definition.)
>
>&#8194;&#8195;The alignment of data behind variable length data elements shall be 8, 16, 32, 64, 128 or 256. Bits.

&#8194;&#8195;SOME/IP 协议通常使用 4 字节对齐方式进行数据传输。这意味着每个字段的长度应该是 4 的倍数。这种对齐方式的主要目的是提高传输效率，因为在很多处理器架构中，4 字节对齐是最优的方式，可以在内存中更快地访问数据。此外，使用 4 字节对齐方式还可以确保字段的偏移量是整数，避免了在解析数据时出现未对齐数据的问题。

&#8194;&#8195;在进行 SOME/IP 序列化时，对于每个结构体中的字段，需要根据其数据类型和对齐要求计算其对齐偏移量。对于大多数数据类型，SOME/IP 协议都要求其对齐偏移量是 4 的倍数。例如，对于一个 8 字节的 double 类型字段，其对齐偏移量应该是 4 的倍数，即 0、4、8、12 等。如果字段的大小不是 4 的倍数，那么需要在其后添加额外的填充字节，以便满足对齐要求。

&#8194;&#8195;对于不同的CPU，数据的存放有不同的对齐原则，有 8、16、32 甚至 64 位对齐（可以配置）。如果一个数据是按照 CPU 对齐的，那么在反序列化的时候会有一定的性能优势。但是 SOME/IP 序列化的时候只支持对动态数据类型自动添加填充位（即动态数组、动态字符串）。使用场景比较局限且序列化的时候还会消耗一些性能，还有一种场景是使用一条 TCP/UDP 报文承载多个 SOME/IP 报文时，原本对齐的数据也可能被破坏。

&#8194;&#8195;有些项目实际使用的是 1 字节对齐，即不对齐。因为 1 字节对齐是最简单的对齐方式，大多编译器很容易实现；并且采用 1 字节对齐，序列化后没有冗余数据，报文的有效负载段都是有意义的数据，所以总体传输效率得到了一定提升。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Serialization_Padding_Example_1.png width="480px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Serialization_Padding_Example_2.png width="480px">

#### 2.2.4.1 &#8194;Basic Datatypes

&#8194;&#8195;按照定义的字节序（即大小端）进行序列化。（简单描述，即基础数据的序列化逐个存放，其他具体数据类型则存在一定规则。）

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Serialization_Basic_Data_Types.png width="600px">

#### 2.2.4.2 &#8194;Structured Datatypes (structs)

>&#8194;&#8195;The serialization of a struct shall be close to the in-memory layout. This means, only the parameters shall be serialized sequentially into the buffer. Especially for structs it is important to consider the correct memory alignment.

&#8194;&#8195;结构体 (struct) 是由一系列具有**相同类型**或**不同类型**的数据构成的数据集合。

&#8194;&#8195;如图示，可根据配置在每段结构体前插入长度为 8 / 16 / 32 Bit 的长度字段（Length Field）。

&#8194;&#8195;Length Field 指定了每段结构体的传输字节数，当长度不匹配，遵循如下原则（长则忽略，短则中止）：

>&#8194;&#8195;If the length is greater than the length of the struct as specified in the data type definition only the bytes specified in the data type shall be interpreted and the other bytes shall be skipped based on the length field.
>
>&#8194;&#8195;If the length is less than the sum of the lengths of all struct members and no substitution for the missing data can be provided locally by the receiver, the deserialization shall be aborted and the message shall be treated as malformed.

&#8194;&#8195;结构体序列化遵循深度优先遍历。（Depth First Search，DFS）

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Struct.png width="600px">

#### 2.2.4.3 &#8194;Structured Datatypes and Arguments with Identifier and optional members

>&#8194;&#8195;To achieve enhanced forward and backward compatibility, an additional Data ID can be added in front of struct members or method arguments.
>
>&#8194;&#8195;The receiver then can skip unknown members/arguments, i.e. where the Data ID is unknown.
>
>&#8194;&#8195;New members/arguments can be added at arbitrary positions when Data IDs are transferred in the serialized byte stream.
>
>&#8194;&#8195;Moreover, the usage of Data IDs allows describing structs and methods with optional members/arguments. Whether a member/argument is optional or not, is defined in the data definition.
>
>&#8194;&#8195;In addition to the Data ID, a wire type encodes the datatype of the following member. Data ID and wire type are encoded in a so-called tag.
>
>&#8194;&#8195;The tag shall consist of
>
>&#8194;&#8194;&#8195;- reserved (Bit 7 of the first byte)
>
>&#8194;&#8194;&#8195;- wire type (Bit 6-4 of the first byte)
>
>&#8194;&#8194;&#8195;- Data ID (Bit 3-0 of the first byte and bit 7-0 of the second byte)

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Tag_Layout.png width="600px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Wire_Type.png width="360px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Example_01_for_serializing_structures_with_tags.png width="600px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Example_02_for_serializing_structures_with_tags.png width="600px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Example_03_for_serializing_structures_with_tags.png width="600px">

#### 2.2.4.4 &#8194;Strings

&#8194;&#8195;包含**固定长度**和**可变长度**字符串。

&#8194;&#8195;支持不同的编码方式 —— UTF-8 / UTF-16(BE) / UTF-16(LE) （其中，BE 表示大端，LE 表示小端）。

&#8194;&#8195;所有字符串以字节顺序标记（Byte Order Mark / BOM）开始，以区分编码类型：

&#8194;&#8194;&#8195;- UTF-8，3 byte BOM，表示为 EF BB BF；

&#8194;&#8194;&#8195;- UTF-16(BE)，2 byte BOM，表示为 FE FF；

&#8194;&#8194;&#8195;- UTF-16(LE)，2 byte BOM，表示为 FF FE；

&#8194;&#8195;所有字符串均以 "\0" 标志结束：

&#8194;&#8194;&#8195;- UTF-8，1 byte character，i.e. 0x00；

&#8194;&#8194;&#8195;- UTF-16(BE/LE)，2 byte character，i.e. 0x00 0x00；

&#8194;&#8195;**Strings (fixed length)**

&#8194;&#8194;&#8195;- 一般而言，String 的固定长度是由数据类型定义所制定，应考虑包含 BOM 和 "\0" 的长度。

&#8194;&#8194;&#8195;- 如果固定长度字符串的长度大于预期，反序列化将被中止，报文将被视为格式错误。

&#8194;&#8194;&#8195;- 如果固定长度字符串的长度小于预期，并且使用 "\0" 正确结束，则接受该字符串；若未使用 "\0" 正确结束，反序列化将被中止，报文将被视为格式错误。

>&#8194;&#8194;&#8195;Instead of transferring application strings as SOME/IP strings with BOM and "\0" termination, strings can also be transported as plain dynamic length arrays without BOM and "\0" termination. Please note that this requires the full string handling (e.g. endianness conversion) to be done in the applications.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-String_Fixed_Length.png width="640px">

&#8194;&#8195;**Strings (dynamic length)**

&#8194;&#8194;&#8195;- 动态长度字符串需要在 BOM 前添加 Length Filed 字段，Length Filed 字段表示长度应包含 BOM 和 "\0" 字节数，但不包括自身。

&#8194;&#8194;&#8195;- Length Field 字段字节数是可配置的 —— 8 / 16 32 Bits，默认 32 Bits。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-String_Dynamic_Length.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-String_Compare.png width="680px">

#### 2.2.4.5 &#8194;Arrays

&#8194;&#8195;数组，指包含多个相同数据类型成员。

&#8194;&#8195;同样分为**固定长度**和**可变长度**数组，同时，需要考虑数组维度。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Array_Compare.png width="700px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-One-dimensional_array_Fixed_Length.png width="480px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Multidimensional_array_Fixed_Length.png width="480px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-One-dimensional_array_Dynamic_Length.png width="480px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Multidimensional_array_Dynamic_Length.png width="480px">

#### 2.2.4.6 &#8194;Enumeration

>&#8194;&#8195;Enumerations are not considered in SOME/IP. Enumerations shall be transmitted as unsigned integer datatypes.

#### 2.2.4.7 &#8194;Bitfield

>&#8194;&#8195;Bitfields shall be transported as unsigned datatypes uint8/uint16/uint32/uint64.
>
>&#8194;&#8195;The data type definition will be able to define the name and values of each bit.

#### 2.2.4.8 &#8194;Union / Variant

>&#8194;&#8195;There are use cases for defining data as unions on the network where the payload can be of different data types.
>
>&#8194;&#8195;A union (also called variant) is such a parameter that can contain different types of data. For example, if one defines a union of type uint8 and type uint16, the union shall carry data which are a uint8 or a uint16.
>
>&#8194;&#8195;**Which data type will be transmitted in the payload can only be decided during execution.** In this case, however, it is necessary to not only send the data itself but add an information about the applicable data type as a form of "meta-data" to the transmission.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Default_Serialization_of_Unions.png width="640px">

&#8194;&#8195;一个 Union 应由三部分构成：Length Field，Type Filed，Payload。

&#8194;&#8194;&#8195;- Length Field 字段长可在 0 / 8 / 16 / 32 Bits 中配置。

>&#8194;&#8194;&#8195;A length of the length field of 0 Bit means that no length field will be written to the PDU.
>
>&#8194;&#8194;&#8195;If the length of the length field is 0 Bit, all types in the union shall be of the same length.

&#8194;&#8194;&#8195;- Length Field 指定 Payload 长度（包括 Data 及 Padding Data），不包含 Length Field 自身以及 Type Filed 字节。

&#8194;&#8194;&#8195;- Type Filed 字段长可在 8 / 16 / 32 Bits 中配置，用于指明数据类型，字段值由配置定义（各 Union 可以分别定义），Type Filed = 0 表示 Empty Union。

&#8194;&#8194;&#8195;- Payload 部分的序列化由 Type Filed 所指定的数据类型所决定。

&#8194;&#8194;&#8195;- 若数据长度大于预期，正常序列化，根据 Length Field 跳过多余字节。

&#8194;&#8194;&#8195;- 若数据长度小于预期，根据数据类型，以决定有效数据是否可以被反序列化，或应视为格式错误而终止反序列化。

>&#8194;&#8195;In the following example a length of the length field is specified as 32 Bits. The union shall support a uint8 and a uint16 as data. Both are padded to the 32 bit boundary (length = 4 Bytes).

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Union_unit8.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Union_unit16.png width="640px">

## 2.3 &#8194;Specification of SOME/IP Protocol

### 2.3.1 &#8194;Transport Protocol Bindings

>&#8194;&#8195;All Transport Protocol Bindings shall support transporting more than one SOME/IP message in a Transport Layer PDU (i.e. UDP packet or TCP segment).
>
>&#8194;&#8195;Each SOME/IP payload shall have its own SOME/IP header.
>
>&#8194;&#8195;One Service-Instance can use the following setup for its communication of all the methods, events, and notifications:
>
>&#8194;&#8194;&#8195;- up to one TCP connection
>
>&#8194;&#8194;&#8195;- up to one UDP unicast connection
>
>&#8194;&#8194;&#8195;- up to one UDP multicast connection

&#8194;&#8195;SOME/IP 支持 TCP / UDP 传输。

&#8194;&#8195;**TCP Binding 部分规则如下：**

&#8194;&#8194;&#8195;- 当 TCP 连接丢失时，未完成的请求应作为超时处理。

&#8194;&#8194;&#8195;- Clinet 应在需要传输首个服务调用或尝试接收第一个 Notification 时打开 TCP 连接。

&#8194;&#8194;&#8195;- Client 负责在连接失败时重新建立 TCP 连接，且应在不再需要 TCP 连接时关闭 TCP 连接；当所有使用 TCP 连接的服务不再可用（停止或超时）时，客户端应关闭 TCP 连接。

&#8194;&#8194;&#8195;- Server 在停止所有服务时不应关闭 TCP 连接，给 Clinet 足够的时间来处理控制数据以自行关闭 TCP 连接。

&#8194;&#8194;&#8195;- 当 Server 在 Client 尚未认识到 TCP 不再需要之前关闭 TCP 连接时，Client 将尝试重新建立 TCP 连接。

&#8194;&#8194;&#8195;- Allowing resync to TCP stream using Magic Cookies.  (see in # [2.2.3.1](#2231-message-id--32-bit))

>&#8194;&#8195;**Multiple Service-Instances**
>
>&#8194;&#8194;&#8195;- Service-Instances of the same Service are identified through different Instance IDs. It shall be supported that multiple Service-Instances reside on different ECUs as well as multiple Service-Instances of one or more Services reside on one single ECU.
>
>&#8194;&#8194;&#8195;- While several Service-Instances of different Services shall be able to share the same port number of the transport layer protocol used, multiple Service-Instances of the same Service on one single ECU shall use different ports per Service-Instance.
>
>&#8194;&#8194;&#8195;- **Rationale: While Instance IDs are used for Service Discovery, they are not contained in the SOME/IP header.**
>
>&#8194;&#8194;&#8195;- A Service Instance can be identified through the combination of the Service ID combined with the socket (i.e. IP address, transport protocol (UDP/TCP), and port number).
>
>&#8194;&#8194;&#8195;- It is recommended that instances use the same port number for UDP and TCP. If a service instance uses UDP port x, only this instance of the service and not another instance of the same service should use exactly TCP port x for its services.

&#8194;&#8195;**Transporting large SOME/IP messages of UDP (SOME/IP-TP)**, see in # [3. SOME/IP SD Protocol Specification](3-SOME--IP-SD-Protocol-Specification)

### 2.3.2 &#8194;Error Handling

# 3 &#8194;SOME/IP SD Protocol Specification

# 4 &#8194;SOME/IP TP Protocol Specification





