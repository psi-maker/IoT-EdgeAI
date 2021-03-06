![IoT Structure](IoTstruct.GIF)  

通信对物联网来说十分常用且关键，在物联网协议中，我们一般分为两大类。
* 传输协议: 一般负责子网内设备间的组网及通信；
* 通信协议: 主要是运行在传统互联网TCP/IP协议之上的设备通讯协议，负责设备通过互联网进行数据交换及通信。  
选择传输和通信协议需要注意以下几点：  
1. 客户对架构耦合程度的需求
2. 速度，可靠性，安全性，开发复杂度的均衡
3. 功耗
4. 可扩展性
5. 现有架构的利用程度  

# 常见传输协议
1. 蓝牙  
兼容的蓝牙IoT传感器非常适合需要短距离连接和低功率通信的应用。蓝牙协议的有效范围为50到100米，支持高达1 Mbps的数据传输速率。最近，物联网开发人员已经表现出对基于蓝牙智能协议的低能耗蓝牙低功耗(BLE)的倾向。与前一代产品相比，BLE的功耗显着降低，但不适合大型文件传输。
2. Zigbee  
基于IEEE 802.15.4标准的Zigbee已成为嵌入式应用中使用最广泛的通信协议之一。Zigbee用于连接10-100米范围内的设备，支持高达250 Kbps的数据速率。作为一种低功耗，低数据速率技术，Zigbee非常适合物联网传感器和物联网网关设备之间的双向数据传输，以及ad hoc无线网状网络。通过其网状拓扑，Zigbee设备可以通过中间设备在一定距离上传输数据。对于在消费和工业领域需要低成本和低功耗传感器网络的物联网应用，Zigbee是一个很好的选择。  
Zigbee协议还包括由128位加密密钥和加密帧定义的安全框架。
3. 6LoWPAN  
6LoWPAN是一种简单的无线网状技术，可使各个节点支持IP。其目标是克服将所有类型的设备连接到互联网的技术和商业障碍。6LoWPan规范还定义了通过IEEE 802.15.4网络交换IPv6数据包的封装和报头压缩机制。6LoWPan集成了安全模块和ACL密钥等安全组件，以及可选的TLS。对于需要低功耗无线通信的小型设备而言，它是一种可行的选择。
4. Wi-Fi  
Wi-Fi旨在取代以太网，并通过IEEE 802.11标准系列提供易于使用的短距离无线连接和跨厂商互操作性。Wi-Fi以更快，更大容量的通信而闻名，并且可以使用2.4 GHz和5 GHz频带在50 m范围内进行传输。由于现有基础设施的普遍存在，其受欢迎程度不断提高。
5. 蜂窝  
需要长距离连续连接的物联网应用可以基于GSM，LTE，EDGE，3G，4G和5G等蜂窝网络进行设计。蜂窝网络可以与设备通信，最远距离可达35公里。蜂窝技术有利于物联网应用，因为它具有以下特点：  
* 通过像Cat-0和Cat-1等LTE网络，物联网应用的成本优化，增强覆盖。
* 通过TLS / DTLS安全性和LTE网络的空中加密实现端到端安全性。
* 最低或零监管法规。使用蜂窝网络，数据可以高达23 dBm进行交换。
6. ModBus  
Modbus协议是一种强大的通信标准，广泛应用于工业自动化和SCADA系统，以便将仪表，传感器和执行器的信号发送回主控制器。Modbus具有广泛的通信协议，可在各种物理链路上运行。Modbus是一种基于主从模型的串行通信协议。使用Modbus的主要优点是它是一个简单的开源协议。Modbus的开发成本很低，并且需要最少的硬件设计。此外，Modbus还支持与各种设备(来自不同供应商)和系统的互操作性和兼容性。
7. PROFINET  
PROFINET广泛用于工业自动化解决方案，用于连接制造环境中的系统。根据IEC 61158和IEC 61784标准，PROFINET以固定的时间间隔(1 ms或更短)提供数据，而不会造成质量损失。它还支持现有的IT标准。 PROFINET与现场总线技术高度兼容，可轻松与现有工业系统集成。PROFINET规范使用指定的数据映射系统定义代理(代理地址)，以允许协议与现代IoT协议进行通信。
8. EtherCAT  
基于CANopen协议和以太网，专门针对工业自动化进行了优化。它允许任何标准PC用作EtherCAT主站，并使用任何拓扑与EtherCAT从站通信。它们可以在30微秒内以高达1,000个I / O点的速率连接工厂车间的所有设备。EtherCAT可靠且速度快，因为消息可以在转发到下一个从系统之前借助专用高性能硬件进行处理。

# 常见通信协议
![IoT Communication Protocol](protocol.GIF)
1. HTTP 松耦合服务调用 
> REST-Representational State Transfer 表述性状态传递，基于HTTP协议开发的一种通信风格。  
>> 适用范围：REST/HTTP主要为了简化互联网中的系统架构，快速实现客户端和服务器之间交互的松耦合，降低了客户端和服务器之间的交互延迟。因此适合在物联网的应用层面，通过REST开放物联网中资源，实现服务被其他应用所调用。  
>> 特点：  
>>> * REST 指的是一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就是RESTful。
>>> * 客户端和服务器之间的交互在请求之间是无状态的。
>>> * 在服务器端，应用程序状态和功能可以分为各种资源，它向客户端公开，每个资源都使用 URI 得到一个唯一的地址。所有资源都共享统一的界面，以便在客户端和服务器之间传输状态。
>>> * 使用的是标准的 HTTP 方法，比如：GET、PUT、POST 和 DELETE。  
>> 示例 
>>> * 手机端：需要终端软件支持，设备端上线时或者访问服务端参数等内容时需要模拟HTTP协议(C语言)向服务器发起请求，而请求的格式一般不使用HTML，而是使用较为简单的XML或者JSON协议格式。  
>>> * 云端对手机端推送：云端使用JSP/PHP等技术开发设计前端网页和简单的逻辑即可；云端使用HttpServlet(即使用http协议的servlet)对设备的HTTP请求进行响应，回复XML或者JSON格式的消息。  
>> 缺点：这种方式通信方式的特点就是一请求一响应，总是要客户端向服务器发出请求，服务器才给予响应。服务器从来都不会主动给客户端发消息，而且在客户端发出请求后，服务器也只是回复一次。这种HTTP单向通信方式在互联网领域发挥巨大的作用，就是服务器端可以是无状态的，极大地简化了服务器的服务流程，提高效率。但在物联网领域，我们要求的是双向的通信能力。服务端要能主动给设备端或者手机发出消息。   

> Ajax技术是浏览器支持的一种JavaScript技术。其能够局部改善用户体验技术，让用户在不察觉浏览器页面刷新的情况向服务器发出请求，并获得响应。本质还是遵守HTTP单向通信的规则，只是页面交互时不需要刷新整个页面。双向通信实时性问题依然未能解决。 
* 客户端发出URL页面请求，服务器响应HTML页面内容。  
* HTML页面使用js调用XMLHttpRequest来向服务器发出异步通信请求。
* 服务器响应XML格式数据给浏览器页面。  
* HTML页面使用DOM模型来动态刷新页面元素  
> Websocket: 是HTML5支持的一种新的协议，能够真正支持浏览器和服务器之间进行双向通信。
2. CoAP协议: Constrained Application Protocol，受限应用协议，应用于无线传感网中协议。 通讯机制为request/response  
适用范围：CoAP是简化了HTTP协议的RESTful API，CoAP是6LowPAN协议栈中的应用层协议，它适用于在资源受限的通信的IP网络。  
Simple Media Control Protocol (SMCP) is a CoAP stack that's used in embedded environments. SMCP is also C-based.  
3. MQTT协议(低带宽): Message Queuing Telemetry Transport，消息队列遥测传输，由IBM开发的即时通讯协议，相比来说比较适合物联网场景的通讯协议。MQTT协议采用发布/订阅模式，所有的物联网终端都通过TCP连接到云端，云端通过主题的方式管理各个设备关注的通讯内容，负责将设备与设备之间消息的转发。  
适用范围：在低带宽、不可靠的网络下提供基于云平台的远程设备的数据传输和监控。  
4. DDS协议(高可靠性、实时): Data Distribution Service for Real-Time Systems，面向实时系统的数据分布服务。 It is a middleware standard that can directly publish or subscribe communications in real time in embedded systems.    
适用范围：分布式高可靠性、实时传输设备数据通信。目前DDS已经广泛应用于国防、民航、工业控制等领域。  
5. AMQP协议(互操作性): Advanced Message Queuing Protocol，先进消息队列协议，用于业务系统例如PLM，ERP，MES等进行数据交换。  类似MQTT,也使用publish/subscribe机制
适用范围：最早应用于金融系统之间的交易消息传递，在物联网应用中，主要适用于移动手持设备与后台数据中心的通信和分析。  
6. XMPP协议(即时通信): Extensible Messaging and Presence Protocol，可扩展通讯和表示协议，一个开源形式组织产生的网络即时通信协议。  
适用范围：即时通信的应用程序，还能用在网络管理、游戏、远端系统监控等。  
7. JMS： Java Message Service，即消息服务，JAVA平台中著名的消息队列协议。  
Java消息服务应用程序接口，是一个Java平台中关于面向消息中间件(MOM)的API，用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。Java消息服务是一个与具体平台无关的API，绝大多数MOM提供商都对JMS提供支持。  
8. STOMP: Simple/Streaming Text Oriented Messaging Protocol is a text-based protocol. However, STOMP does not deal with queues and topics; it uses a send semantic with a destination string.  
9. SSI (Simple Sensor Interface) is a communications protocol for data transfer between a combination of computers and sensors.  
  
Example: 智能家居
* 智能家居中智能灯光控制，可以使用XMPP协议控制灯的开关；对电器的即时控制
* 智能家居的电力供给，发电厂的发动机组的监控可以使用DDS协议；工厂重要区域的实时检测
* 当电力输送到千家万户时，电力线的巡查和维护，可以使用MQTT协议；厂区外非实时性检测
* 家里的所有电器的电量消耗，可以使用AMQP协议，传输到云端或家庭网关中进行分析；数据查看和交换
* 最后用户想把自家的能耗查询服务公布到互联网上，那么可以使用REST/HTTP来开放API服务。 松耦合应用

# MQ Telemetry Transport  
Quote: https://internetofthingsagenda.techtarget.com/definition/MQTT-MQ-Telemetry-Transport  

MQTT was previously known as the SCADA protocol, MQ Integrator SCADA Device Protocol (MQIsdp) and WebSphere MQTT (WMQTT), although all these variations have fallen out of use.  

TT in MQTT stands for Telemetry Transport, the MQ is in reference to a product called IBM MQ. It is a good choice for wireless networks that experience varying levels of latency due to occasional bandwidth constraints or unreliable connections. MQTT has two objects:  
* a client is the connected devices: the subscriber or the publisher.
* a broker is a server: a message hub.   

When a client wants to send data to a broker, it is called a publish. When the operation is reversed, it is called a subscribe.  
* The broker will buffer messages and push them out to the subscriber when it is back online on the condition that a connection from a subscribing client to a broker is broken.
* The broker can close the connection and send subscribers a cached message with instructions from the publisher if the connection from the publishing client to the broker is disconnected without notice.  

## MQTT workflow
An MQTT session is divided into 4 stages:  
1. **connection**  
A client starts by creating a TCP/IP connection to the broker by using either a standard port(1883 for nonencrypted communication) or a custom port defined by the broker's operators. When creating the connection, it is important to recognize that the server might continue an old session if it is provided with a reused client identity.  

2. **authentication**  
port 8883 is used for encrypted communication. Using SSL/TLS handshakes, the client validates the server certificate and authenticates the server. The client may also provide a client certificate to the broker during the handshake. The broker can use this to authenticate the client. While not specifically part of the MQTT specification, it has become customary for brokers to support client authentication with SSL/TLS client-side certificates.  

Because the MQTT protocol aims to be a protocol for resource-constrained and IoT devices, SSL/TLS might not always be an option and, in some cases, might not be desired. On such occasions, authentication is presented as a cleartext username and password, which are sent by the client to the server -- this, as part of the CONNECT/CONNACK packet sequence. In addition, some brokers, especially open brokers published on the internet, will accept anonymous clients. In such cases, the username and password are simply left blank.  

3. **communication**  
MQTT is a lightweight protocol because all its messages have a small code footprint. Each message consists of a fixed header  
* 2 bytes - an optional variable header
* a message payload that is limited to 256 megabytes (MB) of information and a quality of service (QoS) level.  

During the communication phase, a client can perform publish, subscribe, unsubscribe and ping operations. The publish operation sends a binary block of data -- the content -- to a topic that is defined by the publisher.  

MQTT supports message binary large objects (BLOBs) up to 256 MB in size. The format of the content will be application-specific. Topic subscriptions are made using a SUBSCRIBE/SUBACK packet pair, and unsubscribing is similarly performed using an UNSUBSCRIBE/UNSUBACK packet pair.  

Topic strings form a natural topic tree with the use of a special delimiter character, the forward slash (/). A client can subscribe to -- and unsubscribe from -- entire branches in the topic tree with the use of special wild-card characters. There are two wild-card characters: a single-level wild-card character, the plus character (+); and a multilevel wild-card character, the hash character (#). A special topic character, the dollar character ($), excludes a topic from any root wild-card subscriptions. Typically, $ is used to transport server-specific or system messages.  

Another operation a client can perform during the communication phase is to ping the broker server using a PINGREQ/PINGRESP packet sequence. This packet sequence roughly translates to ARE YOU ALIVE/YES I AM ALIVE. This operation has no other function than to maintain a live connection and ensure the TCP connection has not been shut down by a gateway or router.  

4. **termination**  
When a publisher or subscriber wants to terminate an MQTT session, it sends a DISCONNECT message to the broker and then closes the connection. This is called a graceful shutdown because it gives the client the ability to easily reconnect by providing its client identity and resuming where it left off.  

The broker may send subscribers a message from the publisher that the broker has previously cached if the disconnect happen suddenly without time for a publisher to send a DISCONNECT message. The message, which is called a last will and testament, provides subscribers with instructions for what to do if the publisher dies unexpectedly.  

## Examples to use MQTT
* Facebook uses MQTT for its Messenger app, not only because the protocol conserves battery power during mobile phone-to-phone messaging, but also because the protocol enables messages to be delivered efficiently in milliseconds (ms), despite inconsistent internet connections across the globe.  
* Most major cloud services providers, including Amazon Web Services (AWS), Google Cloud, IBM Cloud and Microsoft Azure, support MQTT.  
* the Carriots, Evrything and ThingWorx IoT platforms support the MQTT protocol.  
* Mosquitto is an open source MQTT broker

## Pros and cons of MQTT
**Advantages** 
* efficient data transmission and quick to implement due to its being a lightweight protocol;  
* low network usage, due to minimized data packets;  
* efficient distribution of data;  
* successful implementation of remote sensing and control;  
* fast and efficient message delivery;  
* usage of small amounts of power, which is good for the connected devices;  
* reduction of network bandwidth  

**Disadvantages**  
* MQTT has slower transmit cycles compared to CoAP.  
* MQTT's resource discovery works on flexible topic subscription, whereas CoAP uses a stable resource discovery system.  
* Security and authentication: MQTT is unencrypted. Instead, it uses TLS/SSL for security encryption. MQTT has minimal authentication features built into the protocol. Usernames and passwords are sent in cleartext, and any form of secure use of MQTT must employ SSL/TLS, which, unfortunately, is not a lightweight protocol.  
Authenticating clients with client-side certificates is not a simple process, and there's no way in MQTT to control who owns a topic and who can publish information on it, except using proprietary, out-of-band means. This makes it easy to inject harmful messages into the network, either willfully or by mistake.  

Furthermore, there's no way for the message receiver to know who sent the original message unless that information is contained in the actual message. Security features that have to be implemented on top of MQTT in a proprietary fashion increase the code footprint and make implementations more difficult.  

* difficult to create a globally scalable MQTT network. Because the MQTT protocol was not designed with security in mind, the protocol has traditionally been used in secure back-end networks for application-specific purposes. MQTT's topic structure can easily form a huge tree, and there's no clear way to divide a tree into smaller logical domains that can be federated. This makes it difficult to create a globally scalable MQTT network because, as the size of the topic tree grows, the complexity increases.  
* lack of interoperability. Because message payloads are binary, with no information as to how they are encoded, problems can arise -- especially in open architectures where different applications from different manufacturers are supposed to work seamlessly with each other.  

## Quality of service levels
QoS refers to an agreement between the sender of a message and the message's recipient. QoS will define the guarantee of delivery in referring to a specific message. QoS acts as a key feature in MQTT, giving the client the ability to choose between three levels of service.  

The three different QoS levels determine how the content is managed by the MQTT protocol. Although higher levels of QoS are more reliable, they have more latency and bandwidth requirements, so subscribing clients can specify the highest QoS level they would like to receive.  

1. The simplest QoS level is unacknowledged service. This QoS level uses a PUBLISH packet sequence; the publisher sends a message to the broker one time, and the broker passes the message to subscribers one time. There is no mechanism in place to make sure the message has been received correctly, and the broker does not save the message. This QoS level may also be referred to as at most once, QoS0 or fire and forget.  

2. The second QoS level is acknowledged service. This QoS level uses a PUBLISH/PUBACK packet sequence between the publisher and its broker, as well as between the broker and subscribers. An acknowledgement packet verifies that content has been received, and a retry mechanism will send the original content again if an acknowledgement is not received in a timely manner. This may result in the subscriber receiving multiple copies of the same message. This QoS level may also be referred to as at least once or QoS1.  

3. The third QoS level is assured service. This QoS level delivers the message with two pairs of packets. The first pair is called PUBLISH/PUBREC, and the second pair is called PUBREL/PUBCOMP. The two pairs ensure that, regardless of the number of retries, the message will only be delivered once. This QoS level may also be referred to as exactly once or QoS2.

## Specifications
MQTT has different specifications depending on the specific version. Version 5.0 superseded the last version of MQTT, version 3.1.1. Some newer specifications, as defined by OASIS, include the following:  
* the use of publish/subscribe message patterns;  
* a mechanism that can notify users when abnormal disconnections occur;  
* the three levels of message delivery: at most once, at least once and exactly once;  
* the minimization of transport overhead and protocol exchanges to reduce network traffic;   
* an agnostic messaging transport referring to the content of the payload.  
etc.  
