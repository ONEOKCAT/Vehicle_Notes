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

&#8194;&#8195;&#8195;揭示了 SOME/IP-SD 协议的两个主要内容 —— **服务发现 / Service Discovery**和**服务发布/订阅 / Service Pulish/Subscribe**

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

&#8194;&#8194;&#8195;- SOA 设计的是“服务”架构，包含了服务（应用程序）和可调用接口；SOME/IP 是协议，打包了“服务接口”，也就是应用程序的对外接口。

&#8194;&#8194;&#8195;- 即，SOA 设计内容中的接口部分，会通过 SOME/IP 传输实现，可以将 SOME/IP 视为 SOA 的中间件。

&#8194;&#8194;&#8195;- 注：由于 SOME/IP 本身也是一种面向服务的协议，所以一般认为 SOME/IP 只不过是一种实现 SOA 可行的协议选择；即，可以通过 SOME/IP 用来实现 SOA，但 SOA 的实现却不一定非得用 SOME/IP。

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
>&#8194;&#8194;&#8195;- a notifier which sends data on change from the provider to the subscribers.
>
>&#8194;&#8194;&#8195;- a getter which can be called by the subscriber to explicitly query the provider for the value.
>
>&#8194;&#8194;&#8195;- a setter which can be called by the subscriber when it wants to change the value on provider side.

&#8194;&#8195;SOME/IP 协议同样定义了三种**通信机制**：

&#8194;&#8194;&#8195;- Request/Response Communication：最常见的通信方式之一，Client 发送请求，Server 给予响应。

&#8194;&#8194;&#8195;- Fire&Forget Communication：Client 发送请求，但无需响应。

&#8194;&#8194;&#8195;- Notification Events：Notification 表明的是发布/订阅的概念，通常，Server 端发布服务，Client 订阅该服务。

&#8194;&#8195;**Strategy for sending notifications**

&#8194;&#8194;&#8195;针对不同用例，常见的策略包括以下：

&#8194;&#8195;&#8195;- 周期性更新 / Cyclic update：按固定周期更新数据，e.g. every 100 ms for safety relevant messages with Alive.

&#8194;&#8195;&#8195;- 变化性更新 / Update on change：只要数值发生改变就会更新数据，e.g. door open.

&#8194;&#8195;&#8195;- 超限性更新 / Epsilon change：当前数据与上一轮数据的差值大于某个设定值时，进行更新。"This concept may be adaptive, i.e. the prediction is based on a history; thus, only when the difference between prediction and current value is greater than epsilon an update is transmitted."

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

&#8194;&#8194;&#8195;- Method，强调的是功能的调用，例如控制空调的打开或者关闭；

&#8194;&#8194;&#8195;- Event，本质就是事件，其特点在于可以不涉及任何参数，例如空调出现某个故障或切换到某个模式；

&#8194;&#8194;&#8195;- Field 与前面两种方式最大的区别在于，其核心是对具体数据的操控，一般来说，这个数据是一个具有实际意义的变量；相比之下，Method 和 Event 主要目的在于执行某些动作，虽然这些动作可能会涉及到对于变量的操控，但在设计时，将变量操作设计为 Field 类型，或许更合理。例如，设定/获取空调当前的温度值。

&#8194;&#8194;&#8195;- 此外，Notifier 在订阅之后应立即返回一个初始值，Event 不涉及。

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
>&#8194;&#8195;The Client ID shall also support being unique in the overall vehicle by having a configurable prefix or fixed value (e.g. the most significant byte of Client ID being the diagnostics address or a configured Client ID for a given application/SW-C).

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

&#8194;&#8195;**SOME/IP 支持 TCP / UDP 传输。**

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

&#8194;&#8195;**TCP Binding 部分规则如下：**

&#8194;&#8194;&#8195;- 当 TCP 连接丢失时，未完成的请求应作为超时处理。

&#8194;&#8194;&#8195;- Clinet 应在需要传输首个服务调用或尝试接收第一个 Notification 时打开 TCP 连接。

&#8194;&#8194;&#8195;- Client 负责在连接失败时重新建立 TCP 连接，且应在不再需要 TCP 连接时关闭 TCP 连接；当所有使用 TCP 连接的服务不再可用（停止或超时）时，客户端应关闭 TCP 连接。

&#8194;&#8194;&#8195;- Server 在停止所有服务时不应关闭 TCP 连接，给 Clinet 足够的时间来处理控制数据以自行关闭 TCP 连接。

&#8194;&#8194;&#8195;- 当 Server 在 Client 尚未认识到 TCP 不再需要之前关闭 TCP 连接时，Client 将尝试重新建立 TCP 连接。

&#8194;&#8194;&#8195;- Allowing resync to TCP stream using Magic Cookies.  (see in # [2.2.3.1](#2231-message-id--32-bit))

#### 2.3.1.1 &#8194;Choosing the transport protocol

>&#8194;&#8195;SOME/IP supports User Datagram Protocol (UDP) and Transmission Control Protocol (TCP). While UDP is a very lean transport protocol supporting only the most important features (multiplexing and error detecting using a checksum), TCP adds additional features for achieving a reliable communication. TCP not only handles bit errors but also segmentation, loss, duplication, reordering, and network congestion.

&#8194;&#8195;对于一些车载应用来说，它们设定了非常短的超时时间以达到快速响应的目的。基于这一点，使用 UDP 传输能更好的满足要求，因为应用程序本身可以处理不太可能发生的错误事件。例如，在一些周期性的数据传输场景中，针对传输故障，更好的策略是等待下一次数据传输，而不是尝试修复上一次传输结果。

>&#8194;&#8195;Guideline：
>
>&#8194;&#8194;&#8195;- Use TCP only if very large chunks of data need to be transported (> 1400 Bytes) and no hard latency requirements in the case of errors exists.
>
>&#8194;&#8194;&#8195;- Use UDP if very hard latency requirements (<100ms) in case of errors is needed.
>
>&#8194;&#8194;&#8195;- Use UDP together with SOME/IP-TP if very large chunks of data need to be transported (> 1400 Bytes) and hard latency requirements in the case of errors exists.
>
>&#8194;&#8194;&#8195;- Try using external transport or transfer mechanisms (Network File System, APIX link, 1722, ...) when they are more suited for the use case. In this case SOME/IP can transport a file handle or a comparable identifier. This gives the designer additional freedom (e.g. in regard to caching).
>
>&#8194;&#8195;The transport protocol used is specified by the interface specification on a per-message basis. Methods, Events, and Fields should commonly only use a single transport protocol.

#### 2.3.1.2 &#8194;Multiple Service-Instances

>&#8194;&#8195;Service-Instances of the same Service are identified through different Instance IDs. It shall be supported that multiple Service-Instances reside on different ECUs as well as multiple Service-Instances of one or more Services reside on one single ECU.
>
>&#8194;&#8195;While several Service-Instances of different Services shall be able to share the same port number of the transport layer protocol used, multiple Service-Instances of the same Service on one single ECU shall use different ports per Service-Instance.
>
>&#8194;&#8195;**Rationale: While Instance IDs are used for Service Discovery, they are not contained in the SOME/IP header.**
>
>&#8194;&#8195;A Service Instance can be identified through the combination of the Service ID combined with the socket (i.e. IP address, transport protocol (UDP/TCP), and port number).
>
>&#8194;&#8195;It is recommended that instances use the same port number for UDP and TCP. If a service instance uses UDP port x, only this instance of the service and not another instance of the same service should use exactly TCP port x for its services.

#### 2.3.1.3 &#8194;Transporting large SOME/IP messages of UDP (SOME/IP-TP)

&#8194;&#8195;See in # [4. SOME/IP TP Protocol Specification](#4-someip-tp-protocol-specification)

### 2.3.2 &#8194;Error Handling

#### 2.3.2.1 &#8194;General

>&#8194;&#8195;Error handling can be done in the application or the communication layer below. Therefore SOME/IP supports two different mechanisms:
>
>&#8194;&#8194;&#8195;- Return Codes in the Response Messages of methods
>
>&#8194;&#8194;&#8195;- Explicit Error Messages

&#8194;&#8195;Response Message: Message Type = 0x80, Return Code ≠ 0x00

&#8194;&#8195;Error Message: Message Type = 0x81, Return Code ≠ 0x00

&#8194;&#8195;see in # [2.2.3.6 Message_Type](#2236-message-type--8-bit)

&#8194;&#8195;see in # [2.2.3.7 Return_Code](#2237-return-code--8-bit)

#### 2.3.2.2 &#8194;Error Handing Rules

>&#8194;&#8195;- The receiver of a SOME/IP message shall not return an error message for events/notifications.
>
>&#8194;&#8195;- The receiver of a SOME/IP message shall not return an error message for fire&forget methods.
>
>&#8194;&#8195;-The receiver of a SOME/IP message shall not return an error message for events/notifications and fire&forget methods if the Message Type is set incorrectly to Request or Response.
>
>&#8194;&#8195;- For Request/Response methods the error message shall copy over the fields of the SOME/IP header (i.e. Message ID, Request ID, and Interface Version) but not the payload. In addition Message Type and Return Code have to be set to the appropriate values.

&#8194;&#8195;- 解析：即只有 Request/Response 机制才会返回 Error Message。

>&#8194;&#8195;- If more detailed error information need to be transmitted, the payload of the Error Message (Message Type 0x81) shall be filled with error specific data, e.g. an exception string. Error Messages shall be sent instead of Response Messages.
>
>&#8194;&#8195;- The recommended layout for the exception message is the following:
>
>&#8194;&#8194;&#8195;1. Union of specific exceptions. At least a generic exception without fields needs to exist. 
>
>&#8194;&#8194;&#8195;2. Dynamic Length String for exception description.

>&#8194&#8195;- Explicit Error Messages shall be used to transport application errors and the response data or generic SOME/IP errors from the provider to the caller of a method.

&#8194;&#8195;- 解析：Retrun Code < 0x20 时，针对 SOME/IP 错误，使用 Error Message；Retrun Code ≥ 0x20 时，针对 "specific errors of services and methods"，使用 Response Message。

#### 2.3.2.3 &#8194;Error Processing Overview

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Message_Validation_and_Error_Handling.png>

>&#8194;&#8195;For SOME/IP messages received over UDP, the following shall be checked:
>
>&#8194;&#8194;&#8195;- The UDP datagram size shall be at least 16 Bytes (minimum size of a SOME/IP message)
>
>&#8194;&#8194;&#8195;- The value of the length field shall be less than or equal to the remaining bytes in the UDP datagram payload
>
>&#8194;&#8195;If one check fails, a malformed error shall be issued.

>&#8194;&#8195;**[PRS_SOMEIP_00195]** SOME/IP messages shall be checked by error processing. This does not include the application based error handling but just covers the error handling in messaging and RPC.

>&#8194;&#8195;When one of the errors specified in **[PRS_SOMEIP_00195]** occurs while receiving SOME/IP messages over TCP, the receiver shall check the TCP connection and shall restart the TCP connection if needed.
>
>&#8194;&#8195;Checking the TCP connection might include the following:
>
>&#8194;&#8194;&#8195;- Checking whether data is received for e.g. other Eventgroups.
>
>&#8194;&#8194;&#8195;- Sending out a Magic Cookie message and waiting for the TCP ACK.
>
>&#8194;&#8194;&#8195;- Reestablishing the TCP connection.

#### 2.3.2.4 &#8194;Communication Errors and Handling of Communication Errors

>&#8194;&#8195;When considering the transport of RPC messages different reliability semantics exist:
>
>&#8194;&#8194;&#8195;- Maybe — the message might reach the communication partner
>
>&#8194;&#8194;&#8195;- At least once — the message reaches the communication partner at least once
>
>&#8194;&#8194;&#8195;- Exactly once — the message reaches the communication partner exactly once

>&#8194;&#8195;While different implementations may implement different approaches, SOME/IP currently achieves "maybe" reliability when using the UDP binding and "exactly once" reliability when using the TCP binding. Further error handling is left to the application.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP-Error_Reliability_Maybe.png width="640px">

# 3 &#8194;SOME/IP-SD Protocol Specification

## 3.1 &#8194;SOME/IP-SD Introduction

>&#8194;&#8195;The main tasks of the Service Discovery Protocol are communicating the availability of functional entities called services in the in-vehicle communication as well as controlling the send behavior of event messages.
>
>&#8194;&#8195;This allows sending only event messages to receivers requiring them (Publish/Subscribe).

&#8194;&#8195;通过 SOME/IP-SD 对服务进行订阅，然后再用 SOME/IP 协议中的 Notification 类型报文，发布订阅内容。

### 3.1.1 &#8194;Protocol purpose and objectives

>&#8194;&#8195;SOME/IP-SD is used to
>
>&#8194;&#8194;&#8195;- Locate service instances.
>
>&#8194;&#8194;&#8195;- Detect if service instances are running.
>
>&#8194;&#8194;&#8195;- Implement the Publish/Subscribe handling.
>
>&#8194;&#8195;Inside the vehicular network service instance locations are commonly known; therefore, **the state of the service instance is of primary concern**. The location of the service (i.e. IP-Address, transport protocol, and port number) are of secondary concern.

&#8194;&#8195;定位服务示例（IP/Port）：FindService / OfferService / StopOfferService 

&#8194;&#8195;检测服务示例运行状态                                                                                                                                                                                           

&#8194;&#8195;实现发布和订阅：SubscribeEventgroup / StopSubscribeEventgroup / SubscribeEventgroupACK / SubscribeEventgroupNACK

### 3.1.2 &#8194;Applicability of the protocol

>&#8194;&#8195;SOME/IP-SD can be used for service discovery in automotive vehicle networks.
>
>&#8194;&#8195;The SOME/IP-SD has the following constraints:
>
>&#8194;&#8194;&#8195;- SOME/IP-SD supports only IP based communication.
>
>&#8194;&#8194;&#8195;- The network communication design has to consider the following limitations, if a client service subscribes with an client service multicast endpoint announced via a Multicast option:
>
>&#8194;&#8195;&#8195;Communication which is based on a point to point connection may not be working properly. (e.g. End to end related communication, IPsec communication)
>
>&#8194;&#8195;&#8195;Initial events have to be used carefully, because all client services subscribed with the same multicast endpoint to the same server service, receive the initial events every time a new client service is subscribed with the same multicast endpoint to the same server service.

### 3.1.3 &#8194;Dependencies to other protocol layer

>&#8194;&#8195;SOME/IP-SD depends on SOME/IP. SOME/IP itself supports both TCP and UDP communications but **SOME/IP-SD is constraint to use SOME/IP only over UDP**.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Dependencies_to_other_protocol_layers.png width="240px">

### 3.1.4 &#8194;Use Cases

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Use_Cases.png width="640px">

### 3.1.5 &#8194;Terms and Definitions

>&#8194;&#8195;A separate server service instance shall be used per network interface **if a service needs to be offered on multiple network interfaces**.
>
>&#8194;&#8195;A separate client service instance shall be used per network interface **if a service needs to be configured to be accessed using multiple different network interfaces**.

## 3.2 &#8194;SOME/IP-SD Message Format

>&#8194;&#8195;SOME/IP-SD messages shall be sent over UDP.

>&#8194;&#8195;The SOME/IP-SD Header Format shall follow:
>
>&#8194;&#8194;&#8195;- **Message ID (Service ID/Method ID) [32 bit]: 0xFFFF 8100**
>
>&#8194;&#8194;&#8195;- Length [32 bit]
>
>&#8194;&#8194;&#8195;- Request ID (Client ID/Session ID) [32 bit]
>
>&#8194;&#8194;&#8195;- **Protocol Version [8 bit]: 0x01**
>
>&#8194;&#8194;&#8195;- **Interface Version [8 bit]: 0x01**
>
>&#8194;&#8194;&#8195;- **Message Type [8 bit]: 0x02 (Notification)** 
>
>&#8194;&#8194;&#8195;- **Return Code [8 bit]: 0x00 (E_OK)**
>
>&#8194;&#8194;&#8195;- Flags [8 bit] (**SOME/IP-SD Header Start**s)
>
>&#8194;&#8194;&#8195;- Reserved [24 bit]
>
>&#8194;&#8194;&#8195;- Length of Entries Array [32 bit]
>
>&#8194;&#8194;&#8195;- Entries Array [variable size]
>
>&#8194;&#8194;&#8195;- Length of Options Array [32 bit]
>
>&#8194;&#8194;&#8195;- Options Array [variable size]

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Header_Format.png width="640px">

>&#8194;&#8195;Service Discovery messages shall have a Client-ID (16 its) set to 0x0000, **since there exists only a single SOME/IP-SD instance.**
>
>&#8194;&#8195;Service Discovery messages shall have a Session-ID (16 Bits) and handle it based on SOME/IP requirements.
>
>&#8194;&#8195;The Session-ID (SOME/IP header) shall be incremented for every SOME/IP-SD message sent.
>
>&#8194;&#8195;The Session-ID (SOME/IP header) shall start with 1 and be 1 even after wrapping.
>
>&#8194;&#8195;The Session-ID (SOME/IP header) shall not be set to 0.
>
>&#8194;&#8195;SOME/IP-SD Session ID handling is done per "communication relation", i.e. multicast as well as unicast per peer.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Message-Example.png width="640px">

### 3.2.1 &#8194;Flags

>&#8194;&#8195;The SOME/IP-SD Header shall start with an 8 Bit field called flags.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Header_Flags.png width="540px">

&#8194;&#8195;**Reboot Flag**

>&#8194;&#8195;The Reboot Flag of the SOME/IP-SD Header shall be set to one for all messages after reboot until the Session-ID in the SOME/IP-Header wraps around and thus starts with 1 again. After this wrap around the Reboot Flag is set to 0.
>
>&#8194;&#8195;The information for the reboot flag and the Session ID shall be kept for multicast and unicast separately.
>
>&#8194;&#8195;The information for the reboot flag and the Session ID shall be kept for every sender-receiver relation (i.e. source address and destination address) separately.
>
>&#8194;&#8195;This means there shall be separate counters for sending and receiving.
>
>&#8194;&#8195;**Sending**
>
>&#8194;&#8194;&#8195;- There shall be a counter for multicast.
>
>&#8194;&#8194;&#8195;- There shall be a separate counter for each peer for unicast.
>
>&#8194;&#8195;**Receiving**
>
>&#8194;&#8194;&#8195;- There shall be a counter for each peer for multicast.
>
>&#8194;&#8194;&#8195;- There shall be a counter for each peer for unicast.

>&#8194;&#8195;The detection of a reboot shall be done as follows (with mthe new values of the current packet from the communication partner and old the last value received before):
>
>&#8194;&#8194;&#8195;- if old.reboot==0 and new.reboot==1 then Reboot detected
>
>&#8194;&#8194;&#8195;- OR
>
>&#8194;&#8194;&#8195;- if old.reboot==1 and new.reboot==1 and old.session_id>=new.session_id then Reboot detected.

&#8194;&#8195;**Unicast Flag**

>&#8194;&#8195;The Unicast Flag of the SOME/IP-SD Header shall be set to Unicast (that means 1) for all SD Messages since this means that receiving using unicast is supported.
>
>&#8194;&#8195;The Unicast Flag is left over from historical SOME/IP versions and is only kept for compatibility reasons. Its use besides this is very limited.

&#8194;&#8195;**Undefined bits within the Flag field shall be set to '0' when sending and ignored on receiving.**

### 3.2.2 &#8194;Entry Format

&#8194;&#8195;Entry 是 SOME/IP-SD 报文的主体内容，用于同步服务实例状态和发布/订阅操作。（对于接收方而言，从这里可“进入”获取服务实例的位置以及可用性）

>&#8194;&#8195;The entries are used to synchronize the state of services instances and the Publish/Subscribe handling.
>
>&#8194;&#8195;The entries shall be processed exactly in the order they arrive.
>
>&#8194;&#8195;The service discovery shall support multiple entries thatare combined in one service discovery message.

>&#8194;&#8195;Two types of entries exist: **A Service Entry Type for Services** and **an Eventgroup Entry Type for Eventgroups**.

>&#8194;&#8195;A Service Entry Type shall be 16 Bytes of size and include the following fields in this order:
>
>&#8194;&#8194;&#8195;- Type Field [uint8]: encodes FindService (0x00), OfferService (0x01) and StopOfferService (0x01)
>
>&#8194;&#8194;&#8195;- Index First Option Run [uint8]: Index of this runs first option in the option array.
>
>&#8194;&#8194;&#8195;- Index Second Option Run [uint8]: Index of this runs second option in the option array.
>
>&#8194;&#8194;&#8195;- Number of Options 1 [uint4]: Describes the number of options the first option run uses.
>
>&#8194;&#8194;&#8195;- Number of Options 2 [uint4]: Describes the number of options the second option run uses.
>
>&#8194;&#8194;&#8195;- Service-ID [uint16]: Describes the Service ID of the Service or Service-Instance this entry is concerned with.
>
>&#8194;&#8194;&#8195;- Instance ID [uint16]: **Describes the Service Instance ID of the Service Instance this entry is concerned with or is set to 0xFFFF if all service instances of a service are meant.**
>
>&#8194;&#8194;&#8195;- Major Version [uint8]: Encodes the major version of the service (instance).
>
>&#8194;&#8194;&#8195;- TTL [uint24]: Describes the lifetime of the entry in seconds.
>
>&#8194;&#8194;&#8195;- Minor Version [uint32]: Encodes the minor version of the service.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Service_Entry_Type.png width="640px">

>&#8194;&#8195;An Eventgroup Entry (Type 2) shall be 16 Bytes of size and include the following fields in this order:
>
>&#8194;&#8194;&#8195;- Type Field [uint8]: encodes SubscribeEventgroup (0x06), StopSubscribeEventgroup (0x06), SubscribeAck (0x07) and SubscribeEventgroupNack (0x07).
>
>&#8194;&#8194;&#8195;- Index of first option run [uint8]: Index of this runs first option in the option array.
>
>&#8194;&#8194;&#8195;- Index of second option run [uint8]: Index of this runs second option in the option array.
>
>&#8194;&#8194;&#8195;- Number of Options 1 [uint4]: Describes the number of options the first option run uses.
>
>&#8194;&#8194;&#8195;- Number of Options 2 [uint4]: Describes the number of options the second option run uses.
>
>&#8194;&#8194;&#8195;- Service-ID [uint16]: Describes the Service ID of the Service or Service Instance this entry is concerned with.
>
>&#8194;&#8194;&#8195;- Instance ID [uint16]: **Describes the Service Instance ID of the Service Instancethis entry is concerned with. The Service Instance ID shall not be set to 0xFFFF for any Instance.**
>
>&#8194;&#8194;&#8195;- Major Version [uint8]: Encodes the major version of the service instance this eventgroup is part of.
>
>&#8194;&#8194;&#8195;- TTL [uint24]: Descibes the lifetime of the entry in seconds.
>
>&#8194;&#8194;&#8195;- Reserved [uint12]: Shall be set to 0x000.
>
>&#8194;&#8194;&#8195;- Counter [uint4]: **Is used to differentiate identical Subscribe Eventgroups of the same subscriber. Set to 0x0 if not used.**
>
>&#8194;&#8194;&#8195;- Eventgroup ID [uint16]: Transports the ID of an Eventgroup.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Eventgroup_Entry_Type.png width="640px">

>&#8194;&#8195;The Major Version of an entry shall match the version of the corresponding Service Interface.
>
>&#8194;&#8195;For Service Discovery on Classic Platform the Major version of SdServerService and SdClientService, respectively, has to match the Interface Version of the corresponding Service Interface.

#### 3.2.2.1 &#8194;Entry 字段解析补充

&#8194;&#8195;**Type 和 TTL 共同确认 Entry 类型。**

&#8194;&#8195;TTL：以秒为单位，若设置为 0xFFFFFF 则表示为重启前永远有效。

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Entry_Type_By_TypeAndTTL.png width="480px">

&#8194;&#8195;**Service ID：服务 ID，用于区分不同服务。**

&#8194;&#8195;**Instance ID：服务实例 ID，用于区分不同服务实例。**

&#8194;&#8194;&#8195;通常情况下，不涉及多个服务实例，e.g. 调用车载摄像头服务时，可能会存在前后左右 4 个，其共用一个 Service ID，此时就需要使用 Instance ID 对其进行区分；类似的还包括空调分区等。（当使用 Eventgroup Entry Type 时，Instance ID 不能设置为 0xFFFF）

&#8194;&#8195;**Eventgroup ID：事件组 ID，区分需要订阅的不同事件组。**

&#8194;&#8195;**Counter**：用于区分同一订阅者发送出的

#### 3.2.2.2 &#8194;Referencing Options from Entries

>&#8194;&#8195;Using the following fields of the entries, options are referenced by the entries:
>
>&#8194;&#8194;&#8195;- Index First Option Run: Index into array of options for first option run. Index 0 means first of SOME/IP-SD packet.
>
>&#8194;&#8194;&#8195;- Index Second Option Run: Index into array of options for second option run. Index 0 means first of SOME/IP-SD packet.
>
>&#8194;&#8194;&#8195;- Number of Options 1: Length of first option run. Length 0 means no option in option run.
>
>&#8194;&#8194;&#8195;- Number of Options 2: Length of second option run. Length 0 means no option in option run.

>&#8194;&#8195;Two different option runs exist: First Option Run and Second Option Run.
>
>&#8194;&#8195;Rationale for the support of two option runs: Two different types of options are expected: options common between multiple SOME/IP-SD entries and options different for each SOME/IP-SD entry. Supporting two different options runs is the most efficient way to support these two types of options, while keeping the wire format highly efficient.

>&#8194;&#8195;Each option run shall reference the first option and the number of options for this run.
>
>&#8194;&#8195;If the number of options is set to zero, the option run is considered empty.
>
>&#8194;&#8195;For empty runs the Index (i.e. Index First Option Run and/or Index Second Option Run) shall be set to zero.
>
>&#8194;&#8195;Implementations shall accept and process incoming SD messages with option run length set to zero and option index not set to zero.

### 3.2.3 &#8194;Options Format

>&#8194;&#8195;Options are used to transport additional information to the entries. This includes for instance the information how a service instance is reachable (IP-Address, Transport Protocol, Port Number).

>&#8194;&#8195;In order to identify the option type every option shall start with:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Specifies the length of the option in Bytes. （不包含 Length 和 Type 字段所占字节）
>
>&#8194;&#8194;&#8195;- Type [uint8]: Specifying the type of the option.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Specifies if the option can be discarded. The discardable flag shall be set to 1 if the option can be discarded by a receiving ECU that does not support this option.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Option_Format.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Option_Format_Type1.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Option_Format_Type2.png width="320px">

#### 3.2.3.1 &#8194;Discardable Flag

&#8194;&#8195;目的：说明 Option 是否可被丢弃。

&#8194;&#8195;要求：Configuration Option 和 Load Balance Option 根据可丢弃情况设置；其他 Option Type 必须设置为 0。

&#8194;&#8195;作用：收到 SD 报文，进行 Option 检查 —— 

&#8194;&#8194;&#8195;- 若收到的 Option 未知（对于接收方而言，Option 未定义或不支持），且 Discardable Flag = 1，可忽略该 Option；

&#8194;&#8194;&#8195;- 若收到的 Option 未知（对于接收方而言，Option 未定义或不支持），且 Discardable Flag = 0：

&#8194;&#8195;&#8195;1. 若被 Find 类型 Entry 引用的是 Endpoint Option 或 Multicast Option，则忽略 Option，但仍然能处理 Entry；若 Find 类型 Entry 引用的是 Configuration Option 或 Load Balance Option，则忽略整个 Entry；

&#8194;&#8195;&#8195;2. 若被 Offer 类型 Entry 引用，应忽略 Entry；

&#8194;&#8195;&#8195;3. 若被 SubscribeEventgroup 类型 Entry 引用，应回复 NACK；

&#8194;&#8195;&#8195;4. 若被 SubscribeEventgroupACK 类型 Entry 引用，可以处理 Entry，但会认定为订阅失败。

#### 3.2.3.2 &#8194;Configuration Option

>&#8194;&#8195;The configuration option is used to transport arbitrary configuration strings. This allows to encode additional information like the name of a service or its configuration.

>&#8194;&#8195;The format of the Configuration Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to the total number of bytes occupied by the configuration option, excluding the 16 bit length field and the 8 bit type flag.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x01.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 1 if the Option can be discarded by the receiver.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- ConfigurationString [dyn length]: Shall carry the configuration string.

>&#8194;&#8195;The Configuration Option shall specify a set of name value-pairs based on the DNS TXT and DNS-SD format.
>
>&#8194;&#8195;The format of the configuration string shall start with a single byte length field that describes the number of bytes following this length field.
After the length field a character sequence with the specified length shall follow.
>
>&#8194;&#8195;After each character sequence another length field and a following character sequence are expected until a length field shall be set to 0x00.
>
>&#8194;&#8195;After a length field is set to 0x00 no characters shall follow.
>
>&#8194;&#8195;A character sequence shall encode a key and optionally a value.
>
>&#8194;&#8195;The character sequences shall contain an equal character ("=", 0x3D) to divide key and value.
>
>&#8194;&#8195;The key shall not include an equal character and shall be at least one non-whitespace character. The characters of "Key" shall be printable USASCII values (0x20-0x7E), excluding "=" (0x3D).
>
>&#8194;&#8195;The "=" shall not be the first character of the sequence.
>
>&#8194;&#8195;For a character sequence without an "=" that key shall be interpreted as present.
>
>&#8194;&#8195;For a character sequence ending on an "=" that key shall be interpreted as present with empty value.
>
>&#8194;&#8195;Multiple entries with the same key in a single Configuration Option shall be supported.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Configuration_Option.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Configuration_Option_Example.png width="640px">

#### 3.2.3.3 &#8194;Load Balancing Option

>&#8194;&#8195;The Load Balancing option is used to prioritize different instances of a service, so thata client chooses the service instance based on these settings. This option will be attached to Offer Service entries.

>&#8194;&#8195;The Load Balancing Option shall carry a **Priority** and **Weight** like the DNS-SRV records, which shall be used for load balancing different service instances.
>
>&#8194;&#8195;When looking for all service instances of a service (Service Instance set to 0xFFFF), the client shall choose the service instance with highest priority that also matches client specific criteria.
>
>&#8194;&#8195;When looking for a specific service instances of a service (Service Instance set to any value other than 0xFFFF), the evaluation of the Load Balancing Option does not apply
>
>&#8194;&#8195;When having more than one service instance with highestpriority (lowest value in Priority field) the service instance shall be chosen randomly based on the weights of the service instances. The probability of choosing a service instance shall be the weight of a service instance divided by the sum of the weights of all considered service instances.
>
>&#8194;&#8195;If an Offer Service entry references no Load Balancing option and several service instances are offered, the client shall handle the service instances without Load Balancing option as though they had the lowest priority.

>&#8194;&#8195;The Format of the Load Balancing Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0005.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x02
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 1 if the Option can be discarded by the receiver.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- Priority [uint16]: Carries the Priority of this instance. Lower value means higher priority.
>
>&#8194;&#8194;&#8195;- Weight [uint16]: Carries the Weight of this instance. Large value means higher probability to be chosen.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Load_Balancing_Option.png width="640px">

#### 3.2.3.4 &#8194;IPv4 Endpoint Option

>&#8194;&#8195;The IPv4 Endpoint Option is used by a SOME/IP-SD instance to signal the relevant endpoint(s). Endpoints include the local IP address, the transport layer protocol (e.g. UDP or TCP), and the port number of the sender. These ports are used for the events and notification events as well.

>&#8194;&#8195;The Format of the IPv4 Endpoint Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0009.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x04.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 0.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- IPv4-Address [uint32]: Shall transport the unicast IP-Address as four Bytes.
>
>&#8194;&#8194;&#8195;- Reserved [uint8]: Shall be set to 0x00.
>
>&#8194;&#8194;&#8195;- Transport Protocol (L4-Proto) [uint8]: Shall be set to the transport layer protocol (ISO/OSI layer 4) based on the IANA/IETF types (0x06: TCP, 0x11: UDP).
>
>&#8194;&#8194;&#8195;- Transport Protocol Port Number (L4-Port) [uint16]: Shall be set to the port of the layer 4 protocol.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-IPv4_Endpoint_Option.png width="640px">

>&#8194;&#8195;The server shall use the IPv4 Endpoint Option with OfferService entries to signal the endpoints it serves the service on. That is upto one UDP endpoint and upto one TCP endpoint.
>
>&#8194;&#8195;The endpoints the server referenced with an Offer Service entry shall also be used as source of events. That is source IP address and source port numbers for the transport protocols in the endpoint option.
>
>&#8194;&#8195;The client shall use the IPv4 Endpoint Option with Subscribe Eventgroup entries to signal the IP address and the UDP and/or TCP port numbers, on which it is ready to receive the events.
>
>&#8194;&#8195;Different provided service instances of the same service on the same ECU shall use different endpoints, so that they can be differentiated by the endpoints. Different services may share the same endpoints.

#### 3.2.3.5 &#8194;IPv6 Endpoint Option

>&#8194;&#8195;The IPv6 Endpoint Option is used by a SOME/IP-SD instance to signal the relevant endpoint(s). Endpoints include the local IP address, the transport layer protocol (e.g UDP or TCP), and the port number of the sender. These ports are used for the events and notification events as well.

&#8194;&#8195;其他描述可参考 # [3.2.3.4](#3234-IPv4-Endpoint-Option)

>&#8194;&#8195;The Format of the IPv6 Endpoint Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0015.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x06.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 0.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- IPv6-Address [uint128]: Shall transport the unicast IP-Address as 16 Bytes.
>
>&#8194;&#8194;&#8195;- Reserved [uint8]: Shall be set to 0x00.
>
>&#8194;&#8194;&#8195;- Transport Protocol (L4-Proto) [uint8]: Shall be set to the transport layer protocol (ISO/OSI layer 4) based on the IANA/IETF types (0x06: TCP, 0x11: UDP).
>
>&#8194;&#8194;&#8195;- Transport Protocol Port Number (L4-Port) [uint16]: Shall be set to the transport layer port(e.g. 30490).

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-IPv6_Endpoint_Option.png width="640px">

#### 3.2.3.6 &#8194;IPv4 Multicast Option

>&#8194;&#8195;The IPv4 Multicast Option is either transmitted by the server service (server service multicast endpoint) or by the client service (client service multicast endpoint):
>
>&#8194;&#8194;&#8195;- If it is transmitted by the server service, then a server announces the IPv4 multicast address, the transport layer protocol (ISO/OSI layer 4), and the port number, to where the multicast-events and multicast-notification-events are transmitted to.
>
>&#8194;&#8194;&#8195;- If it is transmitted by the client service, then a client indicates the IPv4 multicast address, the transport layer protocol (ISO/OSI layer 4), and the port number, where a client expects to receive multicast-events and multicast-notification events.

>&#8194;&#8195;IPv4 Multicast Options shall be referenced by SubscribeEventgroup or by StopSubscribeEventgroup or by SubscribeEventgroupAck entries:
>
>&#8194;&#8194;&#8195;- If it is referenced by a SubscribeEventgroup entry, it describes the client service multicast endpoint (i.e. destination IP address and destination port), where the multicast-events shall be received by the client.
>
>&#8194;&#8194;&#8195;- If it is referenced by a StopSubscribeEventgroup entry, it reflects the intent to stop the subscription of a client which has subscribed before via a client service multicast endpoint (i.e. destination IP address and destination port) to the given event group.
>
>&#8194;&#8194;&#8195;- If it is referenced by a SubscribeEventgroupAck entry, it describes the server service multicast endpoint (i.e. destination IP address and destination port), where a server shall transmit the multicast-events to.

>&#8194;&#8195;The Format of the IPv4 Multicast Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0009.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x14.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 0.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- IPv4-Address [uint32]: Shall transport the multicast IP-Address as four Bytes.
>
>&#8194;&#8194;&#8195;- Reserved [uint8]: Shall be set to 0x00.
>
>&#8194;&#8194;&#8195;- Transport Protocol (L4-Proto) [uint8]: Shall be set to the transport layer protocol (ISO/OSI layer 4) based on the IANA/IETF types (0x11: UDP).
>
>&#8194;&#8194;&#8195;- Transport Protocol Port Number (L4-Port) [uint16]: Shall be set to the port of the layer 4 protocol.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-IPv4_Multicast_Option.png width="640px">

#### 3.2.3.7 &#8194;IPv6 Multicast Option

&#8194;&#8195;其他描述可参考 # [3.2.3.6](#3234-IPv4-Multicast-Option)

>&#8194;&#8195;The Format of the IPv6 Multicast Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0015.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x16.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 0.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- IPv6-Address [uint128]: Shall transport the multicast IP-Address as 16 Bytes.
>
>&#8194;&#8194;&#8195;- Reserved [uint8]: Shall be set to 0x00.
>
>&#8194;&#8194;&#8195;- Transport Protocol (L4-Proto) [uint8]: Shall be set to the transport layer protocol (ISO/OSI layer 4) based on the IANA/IETF types (0x11: UDP).
>
>&#8194;&#8194;&#8195;- Transport Protocol Port Number (L4-Port) [uint16]: Shall be set to the port of the layer 4 protocol.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-IPv6_Multicast_Option.png width="640px">

#### 3.2.3.8 &#8194;IPv4 SD Endpoint Option

>&#8194;&#8195;The IPv4 SD Endpoint Option is used to transport the endpoint (i.e. IP-Address and Port) of the senders SD implementation. This is used to identify the SOME/IP-SD Instance even in cases in which the IP-Address and/or Port Number cannot be used.
>
>&#8194;&#8195;This is used to identify the SOME/IP-SD Instance even in cases in which the IP-Address and/or Port Number cannot be used. A use case would be a proxy service discovery on one ECU which handles the multicast traffic for another ECU.

>&#8194;&#8195;The IPv4 SD Endpoint Option shall be included in any SD message up to 1 time.
>
>&#8194;&#8195;The IPv4 SD Endpoint Option shall be the first option in the options array, if existing.
>
>&#8194;&#8195;The IPv4 SD Endpoint Option shall not be referenced by any SD Entry.
>
>&#8194;&#8195;If the IPv4 SD Endpoint Option is included in the SD message, the receiving SD Service Instance shall use the content of this option instead of the Source IP Address and Source Port.
>
>&#8194;&#8194;&#8195;- This is important for answering the received SD message (e.g. Offer after Find orSubscribe after Offer or Subscribe Ack after Subscribe) as well as the reboot detection (channel based on SD Endpoint Option and not out addresses).

>&#8194;&#8195;The Format of the IPv4 SD Endpoint Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0009.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x24.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 0.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- IPv4-Address [uint32]: Shall transport the unicast IP-Address of SOME/IP-SD as four Bytes
>
>&#8194;&#8194;&#8195;- Reserved [uint8]: Shall be set to 0x00.
>
>&#8194;&#8194;&#8195;- Transport Protocol (L4-Proto) [uint8]: Shall be set to the transport layer protocol of SOME/IP-SD (currently: 0x11 UDP).
>
>&#8194;&#8194;&#8195;- Transport Protocol Port Number (L4-Port) [uint16]: Shall be set to the transport layer port of SOME/IP-SD (currently: 30490).

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-IPv4_SD_Endpoint_Option.png width="640px">

#### 3.2.3.9 &#8194;IPv6 SD Endpoint Option

&#8194;&#8195;其他描述可参考 # [3.2.3.8](#3234-IPv4-SD-Endpoint-Option)

>&#8194;&#8195;The Format of the IPv6 SD Endpoint Option shall be as follows:
>
>&#8194;&#8194;&#8195;- Length [uint16]: Shall be set to 0x0015.
>
>&#8194;&#8194;&#8195;- Type [uint8]: Shall be set to 0x26.
>
>&#8194;&#8194;&#8195;- Discardable Flag [1 bit]: Shall be set to 0.
>
>&#8194;&#8194;&#8195;- Bit 1 to bit 7 are reserved and shall be 0.
>
>&#8194;&#8194;&#8195;- IPv6-Address [uint128]: Shall transport the unicast IP-Address of SOME/IP-SD as 16 Bytes.
>
>&#8194;&#8194;&#8195;- Reserved [uint8]: Shall be set to 0x00.
>
>&#8194;&#8194;&#8195;- Transport Protocol (L4-Proto) [uint8]: Shall be set to the transport layer protocol of SOME/IP-SD (currently: 0x11 UDP).
>
>&#8194;&#8194;&#8195;- Transport Protocol Port Number (L4-Port) [uint16]: Shall be set to the transport layer port of SOME/IP-SD (currently: 30490).

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-IPv6_SD_Endpoint_Option.png width="640px">

### 3.2.4 &#8194;Service Entries

#### 3.2.4.1 &#8194;Find Service Entry

>&#8194;&#8195;The Find Service entry type shall be used for finding service instances and shall only be sent if the current state of a service is unknown (no current Service Offer was received and is still valid).

>&#8194;&#8195;Find Service entries shall set the entry fields in the following way:
>
>&#8194;&#8194;&#8195;- Type shall be set to 0x00 (FindService).
>
>&#8194;&#8194;&#8195;- Service ID shall be set to the Service ID of the service that shall be found.（Service ID 设置为当前想要查询的 Service ID）
>
>&#8194;&#8194;&#8195;- Instance ID shall be set to 0xFFFF, if all service instances shall be returned. It shall be set to the Instance ID of a specific service instance, if just a single service instance shall be returned.（设置为 0xFFFF 表示想要查询所有服务实例，否则设定为想要查询的 Instance ID）
>
>&#8194;&#8194;&#8195;- Major Version shall be set to 0xFF, that means that services with any version shall be returned. If set to value different than 0xFF, services with this specific major version shall be returned only.（设置为 0xFF 表示可接收返回任意主版本的服务，否则指定想要返回的特定主版本）
>
>&#8194;&#8194;&#8195;- Minor Version shall be set to 0xFFFF FFFF, that means that services with anyversion shall be returned. If set to a value  different to 0xFFFF FFFF, services with this specific minor version shall be returned only.（同上述）
>
>&#8194;&#8194;&#8195;- TTL is not used for FindService entries and can be set to an arbitrary value. The field is only defined for backward compatibility, and the value shall be ignored by the receiver of the message.

>&#8194;&#8195;A sender shall not reference Endpoint Options nor Multicast Options in a Find Service Entry.
>
>&#8194;&#8195;A receiver shall ignore Endpoint Options and Multicast Options in a Find Service Entry.
>
>&#8194;&#8195;Other Options (neither Endpoint nor Multicast Options), shall still be allowed to be used in a Find Service Entry.
>
>&#8194;&#8195;When receiving a FindService Entry the Service ID, Instance ID, Major Version, and Minor Version shall match exactly to the configured values to **identify a Service Instance**, except if "any values" are in the Entry (i.e. 0xFFFF for Service ID, 0xFFFF for Instance ID, 0xFF for Major Version, and 0xFFFFFFFF for Minor Version.)（为识别到指定的服务实例，Service ID / Instance ID / Major Version / Minor Version 参数都应匹配）

#### 3.2.4.2 &#8194;Offer Service Entry

>&#8194;&#8195;The Offer Service entry type shall be used to offer a service to other communication partners.

>&#8194;&#8195;Offer Service entries shall set the entry fields in the following way:
>
>&#8194;&#8194;&#8195;- Type shall be set to 0x01 (OfferService).
>
>&#8194;&#8194;&#8195;- Service ID shall be set to the Service ID of the service instance offered.
>
>&#8194;&#8194;&#8195;- Instance ID shall be set to the Instance ID of the service instance that is offered.
>
>&#8194;&#8194;&#8195;- Major Version shall be set to the Major Version of the service instance that is offered.
>
>&#8194;&#8194;&#8195;- Minor Version shall be set to the Minor Version of the service instance that is offered.
>
>&#8194;&#8194;&#8195;- TTL shall be set to the lifetime of the service instance. After this lifetime the service instance shall considered not been offered.
>
>&#8194;&#8194;&#8195;- If TTL is set to 0xFFFFFF, the Offer Service entry shall be considered valid until the next reboot.
>
>&#8194;&#8194;&#8195;- **TTL shall not be set to 0x000000 since this is considered to be the Stop Offer Service Entry.**

>&#8194;&#8195;Offer Service entries shall always reference either an IPv4 or IPv6 Endpoint Option to signal how the service is reachable.（至少引用一个 IPv4 / IPv6 Endpoint Option 以表明服务可达路径）
>
>&#8194;&#8195;For each Transport Layer Protocol needed for the service (i.e. UDP and/or TCP) an IPv4(IPv6) Endpoint option shall be added if IPv4(IPv6) is supported.
>
>&#8194;&#8195;When receiving the initial OfferService Entry the Service ID, Instance ID, Major Version and Minor Version shall match exactly to the configured values to identify a Service Instance, except if "any values" are in the service configuration (i.e. 0xFFFF for Instance ID and 0xFFFFFFFF for Minor Version.)（为识别到指定的服务实例，Service ID / Instance ID / Major Version / Minor Version 参数都应匹配）
>
>&#8194;&#8195;When receiving a subsequent OfferService Entry or a StopOfferService Entry the Service ID, Instance ID, Major Version shall match exactly to the values in the initial OfferService entry to identify a Service Instance.（后续的 OfferService Entry 以及 StopOfferService Entry 的参数也应该完全匹配，以识别指定的服务实例）

#### 3.2.4.3 &#8194;Stop Offer Service Entry

>&#8194;&#8195;The Stop Offer Service entry type shall be used to stop offering service instances.

>&#8194;&#8195;Stop Offer Service entries shall set the entry fields exactlylike the Offer Service entry they are stopping, except:（与 OfferService Entry 参数除 TTL 外完全一致）
>
>&#8194;&#8194;&#8195;- TTL shall be set to 0x000000.

>&#8194;&#8195;A StopOfferService (type 0x01), shall carry, i.e. reference, the same options as the entries trying to stop.

### 3.2.5 &#8194;Eventgroup Entries

#### 3.2.5.1 &#8194;Subscribe Eventgroup Entry

>&#8194;&#8195;The Subscribe Eventgroup entry type shall be used to subscribe to an eventgroup.

>&#8194;&#8195;Subscribe Eventgroup entries shall set the entry fields in the following way:
>
>&#8194;&#8194;&#8195;- Type shall be set to 0x06 (SubscribeEventgroup).
>
>&#8194;&#8194;&#8195;- Service ID shall be set to the Service ID of the service instance that includes the eventgroup subscribed to.
>
>&#8194;&#8194;&#8195;- Instance ID shall be set to the Instance ID of the service instance that includes the eventgroup subscribed to.
>
>&#8194;&#8194;&#8195;- Major Version shall be set to the Major Version of the service instance of the eventgroup subscribed to.
>
>&#8194;&#8194;&#8195;- Eventgroup ID shall be set to the Eventgroup ID of the eventgroup subscribed to.
>
>&#8194;&#8194;&#8195;- TTL shall be set to the lifetime of the subscription.
>
>&#8194;&#8195;&#8195;1. If set to 0xFFFFFF, the Subscribe Eventgroup entry shall be considered valid until the next reboot.
>
>&#8194;&#8195;&#8195;2. TTL shall not be set to 0x000000 since this is considered to be the Stop Offer Service Entry.
>
>&#8194;&#8194;&#8195;- Reserved shall be set to 0x000 until further notice.
>
>&#8194;&#8194;&#8195;- Counter shall be used to differentiate between parallel subscribes to the same eventgroup of the same service (only difference in endpoint). If not used, set to 0x0.

>&#8194;&#8195;SubscribeEventgroup entries shall reference options according to the following rules:
>
>&#8194;&#8194;&#8195;- either up to two IPv4 or up to two IPv6 Endpoint Options (one for UDP, one for TCP)
>
>&#8194;&#8194;&#8195;- either up to one IPv4 Multicast Option or up to one IPv6 Multicast Option (only UDP supported)

>&#8194;&#8195;The network communication design has to consider the following points:
>
>&#8194;&#8194;&#8195;- If the server uses only IP multicast for event transmission over UDP the SubscribeEventgroup entry does not need to reference a UDP Endpoint Option.
>
>&#8194;&#8194;&#8195;- If the server transmits initial events for an Eventgroup the SubscribeEventgroup entry shall reference an according Endpoint Option because of "The initial events shall be transported using unicast from Server to Client".
>
>&#8194;&#8194;&#8195;- If a client service subscribes with an Multicast Option (client service multicast endpoint) and the server transmit initial.

>&#8194;&#8195;When receiving a SubscribeEventgroup or StopSubscribeEventgroup the Service ID, Instance ID, Eventgroup ID, and Major Version shall match exactly to the configured values to identify an Eventgroup of a Service Instance.
>
>&#8194;&#8195;If the server receives a Subscribe Eventgroup entry without a UDP Endpoint Option and the MULTICAST_THRESHOLD for the Eventgroup is not configured with value 1 then SubscribeEventGroupNack shall be sent back to the client.

#### 3.2.5.2 &#8194;Stop Subscribe Eventgroup Entry

>&#8194;&#8195;The Stop Subscribe Eventgroup entry type shall be used to stop subscribing to eventgroups.

>&#8194;&#8195;Stop Subscribe Eventgroup entries shall set the entry fields exactly like the Subscribe Eventgroup entry they are stopping, except:
>
>&#8194;&#8194;&#8195;- TTL shall be set to 0x000000.

>&#8194;&#8195;A Stop Subscribe Eventgroup Entry shall reference the same options the Subscribe Eventgroup Entry referenced. This includes but is not limited to Endpoint and Configuration options.

#### 3.2.5.3 &#8194;Subscribe Eventgroup Acknowledgement (Subscribe Eventgroup Ack) Entry

>&#8194;&#8195;The Subscribe Eventgroup Acknowledgment entry type shall be used to indicate that Subscribe Eventgroup entry was accepted.

>&#8194;&#8195;Subscribe Eventgroup Acknowledgment entries shall set the entry fields in the following way:
>
>&#8194;&#8194;&#8195;- Type shall be set to 0x07 (SubscribeEventgroupAck).
>
>&#8194;&#8194;&#8195;- Service ID, Instance ID, Major Version, Eventgroup ID, TTL, Reserved and Counter shall be the same value as in the Subscribe Eventgroup that is being answered.

>&#8194;&#8195;Subscribe Eventgroup Ack entries referencing events and notification events that are transported via multicast shall reference an IPv4 Multicast Option and/or and IPv6 Multicast Option. The Multicast Options state to which Multicast address and port the events and notification events will be sent to.
>
>&#8194;&#8195;When receiving a SubscribeEventgroupAck or SubscribeEventgroupNack the Service ID, Instance ID, Eventgroup ID, and Major Version shall match exactly to the corresponding SubscribeEventgroup Entry to identify an Eventgroup of a Service Instance.

#### 3.2.5.4 &#8194;Subscribe Eventgroup Negative Acknowledgement (Subscribe Eventgroup Nack) Entry

>&#8194;&#8195;The Subscribe Eventgroup Negative Acknowledgment entry type shall be used to indicate that Subscribe Eventgroup entry was NOT accepted.

>&#8194;&#8195;Subscribe Eventgroup Negative Acknowledgment entries shall set the entry fields in the following way:
>
>&#8194;&#8194;&#8195;- Type shall be set to 0x07 (SubscribeEventgroupAck).
>
>&#8194;&#8194;&#8195;- Service ID, Instance ID, Major Version, Eventgroup ID, Counter, and Reserved shall be the same value as in the Subscribe that is being answered.
>
>&#8194;&#8194;&#8195;- The TTL shall be set to 0x000000.

>&#8194;&#8195;Reasons to not accept a Subscribe Eventgroup include (but are not limited to):
>
>&#8194;&#8194;&#8195;- Combination of Service ID, Instance ID, Eventgroup ID, and Major Version is unknown
>
>&#8194;&#8194;&#8195;- Required TCP-connection was not opened by client
>
>&#8194;&#8194;&#8195;- Problems with the references options occurred
>
>&#8194;&#8194;&#8195;- Resource problems at the Server
>
>&#8194;&#8194;&#8195;- Security association not yet established

>&#8194;&#8195;When the client receives a SubscribeEventgroupNack asanswer on a SubscribeEventgroup for which a TCP connection is required, the client shall check the TCP connection and shall restart the TCP connection if needed.
>
>&#8194;&#8195;involves checking the state of the network security protocol, Rational:
>
>&#8194;&#8194;&#8195;- The server might have lost the TCP connection and the client has not.
>
>&#8194;&#8194;&#8195;- Checking the TCP connection might include the following:
>
>&#8194;&#8195;&#8195;1. Checking whether data is received for e.g. other Eventgroups.
>
>&#8194;&#8195;&#8195;2. Sending out a Magic Cookie message and waiting for the TCP ACK.
>
>&#8194;&#8195;&#8195;3. Reestablishing the TCP connection.

### 3.2.6 &#8194;Usage of Options in Entries

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMEIP_SD-Allowed_Option_Types_for_Entry_Types.png width="640px">

### 3.2.7 &#8194;Endpoint Handling for Services and Events

>&#8194;&#8195;**The Service Discovery shall overwrite IP Addresses and Port Numbers with those transported in Endpoint and Multicast Options** if the statically configured values are different from those in these options.（静态配置数值与 Option 不匹配）
>
>&#8194;&#8195;The IP addresses and port numbers of the Endpoint Options shall also be used for transporting events and notification events.
>
>&#8194;&#8195;In case of UDP the endpoint option shall be used for the source address and the source port of the events and notification events, it is also the address the client can send method requests to.
>
>&#8194;&#8195;In case of TCP the endpoint option shall be used for the IP address and port the client needs to open a TCP connection in order to receive events using TCP.
>
>&#8194;&#8195;SOME/IP shall allow services to use UDP and TCP at the same time.
>
>&#8194;&#8195;Which message is sent by which underlying transport protocol shall be determined by configuration: A Service can use UDP and TCP endpoints at the same time. But per element of the service it shall to be configured whether TCP or UDP is used.
>
>&#8194;&#8195;It needs to be restricted in the configuration which methods and which events are provided over TCP/UDP. **This also means that the same event can not be provided over TCP and UDP.**

#### 3.2.7.1 &#8194;Service Endpoints

>&#8194;&#8195;The referenced Endpoint Options of the Offer Service entries denotes the
>
>&#8194;&#8194;&#8195;- IP Address and Port Numbers the service instance is reachable at the server.
>
>&#8194;&#8194;&#8195;- IP Address and Port Numbers the service instance sends the events from.（OfferService Entry 的 Options 中的 IP 和 Port 表示是 Server 端所使用的）

>&#8194;&#8195;Events of this service instance shall not be sent from any other Endpoints than those given in the Endpoint Options of the Offer Service entries.
>
>&#8194;&#8195;If an ECU offers multiple service instances, SOME/IP messages of these service instances shall be differentiated by the information transported in the Endpoint Options referenced by the Offer Service entries.（多个服务实例通过 OfferService Entry 中的 Endpoint Options 进行区分）

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Service_Endpoint_Example.png width="580px">

#### 3.2.7.2 &#8194;Eventgroup Endpoints

>&#8194;&#8195;The Endpoint Options referenced in the Subscribe Eventgroup entries shall also be used to send unicast UDP or TCP SOME/IP events for this Service Instance.
>
>&#8194;&#8195;Thus the Endpoint Options referenced in the Subscribe Eventgroup entries are the IP Address and the Port Numbers on the client side.（Subscribe Eventgroup Entry 的 Options 中的 IP 和 Port 表示是 Client 端所使用的）
>
>&#8194;&#8195;TCP events are transported using the TCP connection the client has opened to the server before sending the Subscribe Eventgroup entry.
>
>&#8194;&#8195;The initial events shall be transported using unicast from Server to Client.（初始事件应使用单播）
>
>&#8194;&#8195;Subscribe Eventgroup Ack entries shall reference up to 1 Multicast Option for the Internet Protocol used (IPv4 or IPv6).（SubscribeEventgroupAck Entry 最多引用一个 UDP 多播服务）
>
>&#8194;&#8195;The Multicast Option shall be set to UDP as transport protocol.
>
>&#8194;&#8195;The client shall open the Endpoint specified in the Multicast Option referenced by the Subscribe Eventgroup Ack entry as fast as possible to not miss multicast events.（Client 端应该尽快开启 SubscribeEventgroupAck Entry 中引用的 Multicast Option 以避免错过多播事件）

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Service_Eventgroup_Example.png width="580px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Publish_Subscribe_Example.png width="720px">

### 3.2.8 &#8194;Service Discovery Messages

>&#8194;&#8195;All SD Messages shall be sent to SD_PORT, **it is a UDP Port for SD Messages (30490 as default)**.
>
>&#8194;&#8195;SD_PORT shall be used as the source port for SD Unicast/Multicast Messages.
>
>&#8194;&#8195;All unicast SD messages should have SD_PORT as destination port unless the SD Endpoint Option defines a different port.
>
>&#8194;&#8195;All SD multicast messages shall be sent using the SD_MULTICAST_IP
>
>&#8194;&#8195;When receiving Service Discovery messages, the receiver shall ignore Entries of unknown type.

### 3.2.9 &#8194;Service Discovery Communication Behavior

#### 3.2.9.1 &#8194;Startup Behavior

>&#8194;&#8195;For each Service Instance the Service Discovery shall have at least these three phases in regard to sending entries:
>
>&#8194;&#8194;&#8195;- **Initial Wait Phase**
>
>&#8194;&#8194;&#8195;- **Repetition Phase**
>
>&#8194;&#8194;&#8195;- **Main Phase**
>
>&#8194;&#8195;An actual implemented state machine will need more than just states for these three phases. E.g. local services can be still down and remote services can be already known (no finds needed anymore).

>&#8194;&#8195;The service discovery shall enter the Initial Wait Phase for a client service instance when the link on the interface needed for this service instance is up and the client service is requested by the application.
>
>&#8194;&#8195;The service discovery shall enter the Initial Wait Phase for a server service instance when the link on the interface needed for this service instance is up and the server service is available.
>
>&#8194;&#8195;It is possible that the link is up but the service is not yet available on server side.
>
>&#8194;&#8195;Systems has started means here the needed applications and possible external sensors and actuators as well. Basically the functionality needed by this service instance has to be ready to offer a service and finding a service is applicable after some application requires it.

&#8194;&#8195;**Initial Wait Phase**

>&#8194;&#8195;The Service Discovery shall wait based on the INITIAL_DELAY after entering the Initial Wait Phase and before sending the first messages for the Service Instance.
>
>&#8194;&#8195;INITIAL_DELAY shall be defined as a minimum and a maximum delay. The wait time shall be determined by choosing a random value between the minimum and maximum of INITIAL_DELAY.

&#8194;&#8195;**Repetition Phase**

>&#8194;&#8195;After sending the first message the Repetition Phase of this Service Instance/these Service Instances is entered.
>
>&#8194;&#8195;The Service Discovery shall wait in the Repetitions Phase based on REPETITIONS_BASE_DELAY.
>
>&#8194;&#8195;After each message sent in the Repetition Phase the delay is doubled.
>
>&#8194;&#8195;The Service Discovery shall send out only up to REPETITIONS_MAX entries during the Repetition Phase.
>
>&#8194;&#8195;If REPETITIONS_MAX is set to 0, the Repetition Phase shall be skipped and the Main Phase is entered for the Service Instance after the Initial Wait Phase.
>
>&#8194;&#8195;Sending Find entries shall be stopped after receiving the corresponding Offer entries by jumping to the Main Phase in which no Find entries are sent.

&#8194;&#8195;**Main Phase**

>&#8194;&#8195;After the Repetition Phase the Main Phase is being entered for a Service Instance.
>
>&#8194;&#8195;After entering the Main Phase, the provider shall wait 1*CYCLIC_OFFER_DELAY before sending the first offer entry message.
>
>&#8194;&#8195;In the Main Phase Offer Messages shall be sent cyclically if a CYCLIC_OFFER_DELAY is configured.
>
>&#8194;&#8195;After a message for a specific Service Instance the Service Discovery waits for 1*CYCLIC_OFFER_DELAY before sending the next message for this Service Instance.
>
>&#8194;&#8195;For Find entries (Find Service and Find Eventgroup) no cyclic messages are allowed in Main Phase.
>
>&#8194;&#8195;Subscribe EventGroup Entries shall be triggered by Offer entries, which are sent cyclically.

&#8194;&#8195;Service Discovery分为4个阶段：

&#8194;&#8194;&#8195;- Down Phase：服务不可用（服务未注册/网络异常）。

&#8194;&#8194;&#8195;- Initial Wait Phase：服务初始化阶段，不响应收到的 Find/Offer 报文。

&#8194;&#8194;&#8195;- Repetition Phase：重复阶段，按照设置的间隔和次数发送 Find/Offer 报文，到达次数或者收到 Find/Offer 报文后进入 Main Phase。

&#8194;&#8194;&#8195;- Main Phase：Server定时发送 Offer 报文，而 Client 则响应 Offer 报文（进行订阅回复）。

&#8194;&#8195;**Server 端 SD 状态迁移**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Server_SD_State.png width="580px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Server_SD_State_Flow.png width="640px">

&#8194;&#8195;**Down Phase**：没有调用 Application 的 Offer_service 接口发布服务，服务不可用。

&#8194;&#8195;**Initial Wait Phase**：

&#8194;&#8194;&#8195;- 调用 Application 的 Offer_Service 接口发布服务。

&#8194;&#8194;&#8195;- 在此阶段，服务处于初始化的阶段，即使收到外部 Client 的 Find 报文，也不对外发送 Offer 报文。

&#8194;&#8194;&#8195;- 根据配置中的 INITIAL_MIN 和 INITIAL_MAX 配置值，在之间取一个随即值作为 initial phase 阶段的具体时长，如果想要固定这个时长，就将 INITIAL_MIN 和 INITIAL_MAX 配置为同一个值。

&#8194;&#8194;&#8195;- 当到达了 initial phase 阶段的时长后，发送第一帧该服务的 Offer 报文，然后进入 Repetition 阶段。

&#8194;&#8195;**Repetition Phase**：

&#8194;&#8194;&#8195;- 在该阶段，将重复发送 Offer 报文，发送的次数和发送的间隔在配置文件中配置，而且，每一次发送的间隔会比上一次延长一倍。

&#8194;&#8194;&#8195;- 例如，配置了 Repetition 阶段发送 3 次 Offer，发送 DELAY 为 100 ms，则在此阶段的三帧 Offer 报文的发送间隔是：Offer -> 200 ms -> Offer -> 400 ms -> Offer -> 800 ms -> Offer，总发送报文数应该为配置值 + 1。

&#8194;&#8194;&#8195;- 在此阶段收到 Client 发送的 Find 报文，将使用单播通信单独发送 Offer 报文作为回应，这次发送会有一个延时，根据配置中的REQUEST_RESPONSE_DELAY 决定。
 
&#8194;&#8194;&#8195;- 在此阶段收到 Client 发送的 Subscribe 报文后，将使用单播通信发送 Subscribe_ACK/NACK 报文进行回复，并且开启订阅 Entry 的 TTL 计时。

&#8194;&#8194;&#8195;- 当到达了 Repetition phase 的时长后，进入 Main 阶段。

&#8194;&#8195;**Main Phase**：

&#8194;&#8194;&#8195;- 在该阶段，将按照配置的周期进行 Offer 报文发送（在 CYCLIC_OFFER_DELAY 配置项中）。

&#8194;&#8194;&#8195;- 在此阶段收到 Client 发送的 Find 报文，将使用单播通信单独发送 Offer 报文作为回应，这次发送会有一个延时，根据配置中的REQUEST_RESPONSE_DELAY 决定。
 
&#8194;&#8194;&#8195;- 在此阶段收到 Client 发送的 Subscribe 报文后，将使用单播通信发送 Subscribe_ACK/NACK 报文进行回复，并且开启订阅 Entry 的 TTL 计时。

&#8194;&#8195;**Client 端 SD 状态迁移**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Client_SD_State.png width="580px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Client_SD_State_Flow.png width="640px">

&#8194;&#8195;**Down Phase**：没有调用 Application 的 Request_service 接口请求服务。

&#8194;&#8195;**Initial Wait Phase**：

&#8194;&#8194;&#8195;- 调用 Application 的 Request_Service 接口请求服务。

&#8194;&#8194;&#8195;- 此阶段的持续时间和 Server 端的 INITIAL 一致，在配置的 INITIAL_MIN 和 INITIAL_MAXV 之间选取随机值作为该阶段的持续时间。

&#8194;&#8194;&#8195;- 在此阶段，如果收到 Server 端发送的 Offer 报文，则直接跳过 Repetition 阶段，进入 Main 阶段。

​&#8194;&#8194;&#8195;-当到达了 initial 阶段的时长后，将发送第一帧 Find 报文。

&#8194;&#8195;**Repetition Phase**：

&#8194;&#8194;&#8195;- 在此阶段，定周期发送 Find 报文，发送次数由配置中的 REPETITIONS_MAX 参数决定，发送的间隔规律和 Server 端 Repetition 阶段 Offer 报文的发送规律一致。

​&#8194;&#8194;&#8195;- 在此阶段，收到 Server 端的 Offer 报文，则进入 Main 阶段。

​&#8194;&#8194;&#8195;- 在此阶段，收到 Server 端的 Stop Offer 报文，则进入 Main 阶段。

​&#8194;&#8194;&#8195;- 当到达 Repetition 阶段的时长后（发送了配置中的 Find 报文次数），进入 Main 阶段。

&#8194;&#8195;**Main Phase**：

&#8194;&#8194;&#8195;- 在这个阶段，主要发送订阅报文/取消订阅报文，并且不是周期发送，而是在收到 Offer / Stop Offer 报文后发送。

&#8194;&#8194;&#8195;- 在此阶段，收到 Server 发送的 Offer 报文，如果 Client 有订阅其中的事件/事件组，则发送 Subscribe 报文。

&#8194;&#8194;&#8195;- 在此阶段，收到 Server 发送的 Stop Offer 报文，如果 Client 有订阅其中的事件/事件组，则发送 Stop Subscribe 报文。

&#8194;&#8195;**设计目的**

&#8194;&#8195;Initial Wait Phase：启动 ECU 的去偏移事件以避免流量突发，并允许 ECU 收集 SD Message 中的多个 Entries。

&#8194;&#8195;Repetition Phase：该阶段用于 Client 和 Server 的快速同步，如果 Client 启动较晚，它能够很快查询到 Server；如果 Server 稍后启动，则也能很快找到 Client；该阶段通过指数级方式增加两帧报文的间隙，以避免过载情况使系统无法同步。

&#8194;&#8195;Main Phase：进入稳定状态，从而通过不再发送 Find Service 以降低数据包速率，并且仅在循环间隔（e.g. 1s）中提供 SD。

#### 3.2.9.2 &#8194;Server Answer Behavior

>&#8194;&#8195;The Service Discovery shall delay answers to entries that were received in multicast SOME/IP-SD messages using the configuration item REQUEST_RESPONSE_DELAY. This is valid for the following two cases:（需要延时的响应机制）
>
>&#8194;&#8194;&#8195;- Offer entry (unicast or multicast) after received find entry (multicast)（Server：以单播/多播 Offer Entry 响应多播 Find Entry）
>
>&#8194;&#8194;&#8195;- Subscribe entry (unicast) after received offer entry (multicast)（Client：以单播 Subscribe Entry 响应多播 Offer Entry）
>
>&#8194;&#8195;The REQUEST_RESPONSE_DELAY shall not apply（不需要延时的响应机制）
>
>&#8194;&#8194;&#8195;- if unicast messages are answered with unicast messages.（以单播报文响应单播报文）
>
>&#8194;&#8195;The actual delay shall be randomly chosen between minimum and maximum of REQUEST_RESPONSE_DELAY.

>&#8194;&#8195;For basic implementations all Find Service entries shall be answered with Offer Service entries transported using unicast.（一般情况下，所有的 Find Entry 都会被单播 Offer Entry 应答）
>
>&#8194;&#8195;For optimization purpose the following behaviors shall be supported as option:
>
>&#8194;&#8194;&#8195;- Find messages received with the Unicast Flag set to 1 in main phase, shall be answered with a unicast response if the latest offer was sent less than 1/2 CYCLIC_OFFER_DELAY ago.
>
>&#8194;&#8194;&#8195;- Find messages received with the Unicast Flag set to 1 in main phase, shall be answered with a multicast RESPONSE if the latest offer was sent 1/2 CYCLIC_OFFER_DELAY or longer ago.
>
>&#8194;&#8194;&#8195;- Entries received with the unicast flag set to 0, shall not be answered with unicast but ignored.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Optimization_Answer_Behavior.png width="540px">

#### 3.2.9.3 &#8194;Shutdown Behavior

>&#8194;&#8195;When a server service instance of an ECU is in the Repetition and Main Phase and is being stopped, a Stop Offer Service entry shall be sent out.
>
>&#8194;&#8195;When the link goes down for a server service instance in the Initial Wait Phase, Repetition Phase or Main Phase, the service discovery shall enter the Down Phase and reenter into Initial Wait Phase when the link is up again and the service is still available.
>
>&#8194;&#8195;When the link goes down for a client service instance in the Initial Wait Phase, Repetition Phase or Main Phase, the service discovery shall enter the Down Phase and reenter into Initial Wait Phase when the link is up again and the service is still available.
>
>&#8194;&#8195;When a server sends out a Stop Offer Service entry all subscriptions for this service instance shall be deleted on the server side.
>
>&#8194;&#8195;When a client receives a Stop Offer Service entry all subscriptions for this service instance shall be deleted on the client side.
>
>&#8194;&#8195;When a client receives a Stop Offer Service entry, the client shall not send out Find Service entries but wait for Offer Service entry or change of status (application, network management, Ethernet link, or similar).
>
>&#8194;&#8195;When a client service instance of an ECU is in the Main Phase and is being stopped (i.e. the service instance is released), the SD shall send out Stop Subscribe Eventgroup entries for all subscribed Eventgroups.
>
>&#8194;&#8195;When the whole ECUs is being shut down Stop Offer Service entries shall be sent out for all service entries and Stop Subscribe Eventgroup entries for Eventgroups.

#### 3.2.9.4 &#8194;State Machines

&#8194;&#8195;**Server**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-State_Machines_Server.png width="720px">

&#8194;&#8195;**Client**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-State_Machines_Client.png width="720px">

&#8194;&#8195;**Unicast**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-State_Machines_Unicast.png width="720px">

&#8194;&#8195;**Multicast**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-State_Machines_Multicast.png width="720px">

&#8194;&#8195;**Unicast/Multicast self-adapting**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-State_Machines_Unicast_Multicast.png width="720px">

#### 3.2.9.5 &#8194;Error Handling

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Error_Handling.png width="640px">

>&#8194;&#8195;Check the referenced Options of each received entry:
>
>&#8194;&#8194;&#8195;- The referenced options exist.
>
>&#8194;&#8194;&#8195;- The entry references all required options (e.g. a provided eventgroup that uses unicast requires a unicast endpoint option in a received Subscribe Eventgroup entry).
>
>&#8194;&#8194;&#8195;- The entry only references supported options (e.g. a required eventgroup that does not support multicast data reception does not support multicast endpoint options in a Subscribe Eventgroup ACK entry).
>
>&#8194;&#8194;&#8195;- There are no conflicts between the options referenced by an entry (i.e. two options of same type with contradicting content).
>
>&#8194;&#8194;&#8195;- The Type of the referenced Option is known or the discardable flag is set to 1.
>
>&#8194;&#8194;&#8195;- The Type of the referenced Option is allowed for the entry or discardable flag is set to 1.
>
>&#8194;&#8194;&#8195;- The Length of the referenced Option is consistent to the Type of the Option.
>
>&#8194;&#8194;&#8195;- An Endpoint Option has a valid L4-Protocol field.
>
>&#8194;&#8194;&#8195;- The Option is valid (e.g. a multicast endpoint option shall use a multicast IP address).

>&#8194;&#8195;If an entry references an option that is known by the Service Discovery implementation but not required by the service (e.g. an Offer references a TCP and UDP option and the client uses only UDP, or a Subscribe Eventgroup entry references a UDP endpoint option but the server uses only multicast event transmission), the entry shall be processed.
>
>&#8194;&#8195;Check if the TCP connection is already present (only applicable, if TCP is configured for Eventgroup and Subscribe Eventgroup entry was received).
>
>&#8194;&#8195;Check if enough resources are left (e.g. Socket Connections).
>
>&#8194;&#8195;If the checks in "Check the referenced Options of each received entry" fail for a received Find entry, the entry shall be ignored, except when Endpoint or Multicast Options are referenced, in which case only the Options shall be ignored.
>
>&#8194;&#8195;If the checks in "Check the referenced Options of each received entry" fail for a received Offer entry, the entry shall be ignored.
>
>&#8194;&#8195;If the checks in "Check the referenced Options of each received entry" or "Check if the TCP connection" fail for a received Subscribe Eventgroup ACK entry, the entry shall be processed, but the subscription shall not be considered as successful.

>&#8194;&#8195;Options that are referenced by an entry shall be ignored if:
>
>&#8194;&#8194;&#8195;- The Option Type is not known (i.e. not yet specified, or not supported by the receiver) and the discardable flag is set to 1.
>
>&#8194;&#8194;&#8195;- The option is redundant (i.e. another option of the same type and same content is referenced by this entry).
>
>&#8194;&#8194;&#8195;- The option is not required (e.g. a provided eventgroup that uses only multicast does not require a unicast endpoint option in a received Subscribe Eventgroup entry, though it is still allowed).
>
>&#8194;&#8195;If the two Configuration Options have conflicting items (same name), all items shall be handled. There shall be no attempt been made to merge duplicate items.
>
>&#8194;&#8195;Check for a provided service instance which requires a secure connection if on reception of a subscribe the security association for the corresponding connection is already established.

### 3.2.10 &#8194;Non-SOME/IP protocols with SOME/IP-SD

>&#8194;&#8195;Besides SOME/IP other communication protocols are used within the vehicle; e.g. for Network Management, Diagnosis, or Flash Updates. Such communication protocols might need to communicate a service instance or have eventgroups as well.
>
>&#8194;&#8195;For Non-SOME/IP protocols (**the application protocol itself doesn't use SOME/IP but it is published over SOME/IP SD**) a special Service-ID shall be used and further information shall be added using the configuration option:
>
>&#8194;&#8194;&#8195;- Service-ID shall be set to 0xFFFE (reserved).
>
>&#8194;&#8194;&#8195;- Instance-ID shall be used as described for SOME/IP services and eventgroups.
>
>&#8194;&#8194;&#8195;- The Configuration Option shall be added and shall contain exactly one entry with key "otherserv" and a configurable non-empty value that is determined by the system department. (Example for valid otherserv-string: "otherserv=internaldiag".)

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Example_PDU_Non_SOMEIP_SD.png width="720px">

### 3.2.11 &#8194;Publish/Subscribe with SOME/IP and SOME/IP-SD

>&#8194;&#8195;In contrast to the SOME/IP request/response mechanism there may be cases in which a client requires a set of parameters from a server, but does not want to request that information each time it is required. These are called notifications and concern events and fields.（Client 需要来自 Server 一组参数，但不希望每次需要时都进行请求）
>
>&#8194;&#8195;All clients needing events and/or notification events shall register using the SOME/IP-SD at run-time with a server.

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Notification_Interaction.png width="640px">

>&#8194;&#8195;With the SOME/IP-SD entry Offer Service the server offers to push notifications to clients; thus, it shall be used as trigger for Subscriptions.
>
>&#8194;&#8195;Each client shall respond to a SOME/IP-SD Offer Service entry from the server with a SOME/IP-SD Subscribe Eventgroup entry as long as the client is still interested in receiving the notifications/events of this eventgroup.
>
>&#8194;&#8195;If the client is able to reliably detect the reboot of the server using the SOME/IP-SD messages reboot flag, the client may choose to only answer Offer Service messages after the server reboots if configured to do so (TTL set to maximum value). The client make sure that this works reliable even when the SOME/IP-SD messages of the server are lost.
>
>&#8194;&#8195;If the client sends out additional Subscribe Eventgroup entries and the TTL of the previous Subscribe has not expired, then the client shall not request Initial Events.
>
>&#8194;&#8195;If the client subscribes to two or more eventgroups including one or more identical events or fields, the server shall not send duplicated events or notification events for the field. This does mean regular events and not initial events.

>&#8194;&#8195;It is not allowed to request initial values of events upon subscriptions (pure event and not field).（只有 Filed 类型的 Notifier 才会返回初始事件，涉及到初始值的概念，比如空调事件；Event 类型则是没有此概念的）
>
>&#8194;&#8195;Reasons for the client to explicitly request Initial Events include but are not limited to:
>
>&#8194;&#8194;&#8195;- The client is currently not subscribed to the Eventgroup.
>
>&#8194;&#8194;&#8195;- The client has seen a link-down/link-up after the last Subscribe Eventgroup entry.
>
>&#8194;&#8194;&#8195;- The client has not received a Subscribe Eventgroup Ack after the last regular Subscribe Eventgroup.
>
>&#8194;&#8194;&#8195;- The client has detected a Reboot of the Server of this Services.

>&#8194;&#8195;The SOME/IP-SD on the server shall delete the subscription, if a relevant SOME/IP error occurs after sending an event or notification event.
>
>&#8194;&#8195;The error includes but is not limited to not being able to reach the communication partner and errors of the TCP connection.

>&#8194;&#8195;The client shall wait for the Subscribe Eventgroup Ack entry acknowledging a Subscribe Eventgroup entry. If this Subscribe Eventgroup Ack entry does not arrive before the next Subscribe Eventgroup entry is sent, the client shall do the following:
>
>&#8194;&#8194;&#8195;- Send a Stop Subscribe Eventgroup entry and a Subscribe Eventgroup entry in the same SOME/IP-SD message the Subscribe Eventgroup entry would have been sent with.
>
>&#8194;&#8195;This behavior exists to cope with short durations of communication loss, so new Initial Events are triggered to lower the effects of the loss of messages.

>&#8194;&#8195;If a client sends a Subscribe Eventgroup entry as a reaction to a unicast offer, and a multicast offer arrives immediately after that but before the the Subscribe Eventgroup Ack entry could be sent by the server and received, the client shall not complain (i.e. Stop Subscribe/Subscribe) about a not yet received acknowledgement.
>
>&#8194;&#8195;This behavior exists to cope with short durations of communication loss. The receiver of a Stop Subscribe Eventgroup and Subscribe Eventgroup combination would sent out Initial Events to lower the effects of the loss of messages.
>
>&#8194;&#8195;If the initial value is of concern - i.e. for fields - the server shall send the first notifications/events (i.e. initial events) immediately after sending the Subscribe Eventgroup Ack.
>
>&#8194;&#8195;It is not allowed to send initial values of events upon subscriptions (pure event and not field).
>
>&#8194;&#8195;If a subscription was already valid and is updated by a Subscribe Eventgroup entry, no initial events shall be sent.
>
>&#8194;&#8195;Receiving Stop Subscribe / Subscribe combinations trigger initial events of field notifiers.

&#8194;&#8195;**Publish/Subscribe with link loss at client**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Publish_Subscribe_LinkLoss_Client.png width="720px">

&#8194;&#8195;**Publish/Subscribe Registration/Deregistration behavior**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Publish_Subscribe_Registration_Deregistration_Behavior.png width="720px">

&#8194;&#8195;**Publish/Subscribe with link loss at server**

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Publish_Subscribe_LinkLoss_Server.png width="720px">

### 3.2.12 &#8194;Reserved and special identifiers for SOME/IP and SOME/IP-SD

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Reserved_and_Special_Service-IDs.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Reserved_and_Special_Instance-IDs.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Reserved_and_Special_Method-IDs.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Reserved_Eventgroup-ID.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Method-IDs_of_Service_0xFFFF.png width="640px">

<img src=https://github.com/ONEOKCAT/Vehicle_Notes/blob/main/INSET/SOMIP_SD-Reserved_Names.png width="640px">

# 4 &#8194;SOME/IP TP Protocol Specification







