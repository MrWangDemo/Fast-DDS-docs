.. raw:: html

  <h1>
    eProsima Fast DDS Documentation
  </h1>

.. image:: /01-figures/logo.png
  :height: 100px
  :width: 100px
  :align: left
  :alt: eProsima
  :target: http://www.eprosima.com/

*eProsima Fast DDS* is a C++ implementation of the
`DDS (Data Distribution Service) Specification <https://www.omg.org/spec/DDS/About-DDS/>`__, a protocol
defined by the `Object Management Group (OMG) <https://www.omg.org/>`__.
The *eProsima Fast DDS* library provides both an Application Programming Interface (API) and a communication protocol
that deploy
a Data-Centric Publisher-Subscriber (DCPS) model, with the purpose of establishing efficient and reliable
information distribution among Real-Time Systems.
*eProsima Fast DDS* is predictable, scalable, flexible, and efficient in resource handling.
For meeting these requirements, it makes use of typed interfaces and hinges on a many-to-many
distributed network paradigm that neatly allows separation of the publisher and subscriber sides of the communication.
*eProsima Fast DDS* comprises:

eProsima Fast DDS是DDS(数据分发服务)规范的c++实现，
该规范是由对象管理组(OMG)定义的协议。
eProsima Fast DDS库提供了一个应用程序编程接口(API)和一个通信协议，
用于部署以数据为中心的发布者-订阅者(dps)模型，
目的是在实时系统之间建立高效可靠的信息分发。eProsima Fast DDS具有可预测、可扩展、灵活和高效的资源处理功能。为了满足这些需求，它利用了类型化接口，并依赖于多对多的分布式网络范式，该范式允许通信的发布者和订阅者分开。eProsima Fast DDS包括:

#.  The :ref:`DDS API <dds_layer>` implementation. // DDS API 是如何实现的
#.  :ref:`Fast DDS-Gen <fastddsgen_intro>`, a generation tool for bridging typed interfaces with the middleware
    implementation. //用于将类型化接口（idl文件）生成可执行代码的工具
#.  The underlying :ref:`RTPS <rtps_layer>` wire protocol implementation. //有线协议实现


For all the above, *eProsima Fast DDS* has been chosen as the default middleware supported by the
`Robot Operating System 2 (ROS 2) <https://index.ros.org/doc/ros2/>`__ in every long term (LTS) releases and most of the
non-LTS releases.

对于上述所有问题，在每个长期(LTS)版本和大多数非LTS版本中，都选择eProsima Fast DDS作为机器人操作系统2 (ROS 2)支持的默认中间件。

DDS API
^^^^^^^

The communication model adopted by DDS is a many-to-many unidirectional data exchange where the applications that
produce the data publish it to the local caches of subscribers belonging to applications that consume the data.
The information flow is regulated by Quality of Service (QoS) policies established between the entities in
charge of the data exchange.

DDS所采用的通信模型是多对多的单向数据交换生成数据，
并将其发布到属于使用该数据的应用程序的订阅者的本地缓存。
信息流由在实体之间建立的服务质量(QoS)策略控制
负责数据交换。

As a data-centric model, DDS builds on the concept of a "global data space" accessible to all interested applications.
Applications that want to contribute information declare their intent to become publishers, whereas applications that
want to access portions of the data space declare their intent to become subscribers.
Each time a publisher posts new data into this space, the middleware propagates the information to all
interested subscribers.

作为一个以数据为中心的模型，DDS建立在所有感兴趣的应用程序都可以访问的“全局数据空间”的概念之上。
希望提供信息的应用程序声明其意图成为发布者，而那些希望提供信息的应用程序
想要访问数据空间的部分，则声明其成为订阅者的意图。
每当发布者将新数据发布到此空间时，中间件就会将信息传播给所有人
感兴趣的用户。

The communication happens across domains, i. e. isolated abstract planes that link all the distributed applications
able to communicate with each other.
Only entities belonging to a same domain can interact, and the matching between entities subscribing to data and
entities publishing them is mediated by topics. Topics are unambiguous identifiers that associate a
name, which is unique in the domain, to a data type and a set of attached data-specific QoS.

通信是跨域进行的，即连接所有分布式应用程序的孤立抽象平面
能够互相沟通。
只有属于同一个域的实体才能进行交互，而订阅数据的实体之间的匹配是不可避免的
发布它们的实体由主题进行中介。主题是将对象关联起来的明确标识符
一个数据类型和一组附加的特定于数据的QoS。

DDS entities are modeled either as classes or typed interfaces.
The latter imply a more efficient resource handling as knowledge of the data
type prior to the execution allows allocating memory in advance rather than dynamically.

DDS实体被建模为类或类型化接口。
后者意味着作为数据知识的更有效的资源处理
在执行前输入允许提前分配内存，而不是动态分配。

.. figure:: /01-figures/DDS_concept.svg
    :align: center

    Conceptual diagram of how information flows within DDS domains.
    Only entities belonging to the same domain can discover each
    other through matching topics, and consequently exchange data between publishers and subscribers.
    
    信息如何在DDS域中流动的概念图。
    只有属于同一域的实体才能发现它们
    其他通过匹配主题，从而在发布者和订阅者之间交换数据。

Fast DDS-Gen
^^^^^^^^^^^^

Relying on interfaces implies the need for a generation tool that translates type descriptions into appropriate
implementations that fill the gap between the interfaces and the middleware.
This task is carried out by a dedicated generation tool, :ref:`Fast DDS-Gen <fastddsgen_intro>`, a Java application
that generates source code using the data types defined in an
`Interface Definition Language (IDL) <https://www.omg.org/spec/IDL/About-IDL/>`__ file.

依赖于接口意味着需要一个生成工具，
将类型描述转换为适当的实现，
以填补接口和中间件之间的空白。
此任务由专用的生成工具执行，
该工具为, :ref:`Fast DDS-Gen <fastddsgen_intro>`,这是一个Java应用程序，使用接口定义语言(IDL)文件中定义的数据类型生成源代码。

RTPS Wire Protocol
^^^^^^^^^^^^^^^^^^

The protocol used by *eProsima Fast DDS* to exchange messages over standard networks is the `Real-Time
Publish-Subscribe protocol (RTPS) <https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/>`__, an interoperability wire
protocol for DDS defined and maintained by the OMG
consortium.
This protocol provides publisher-subscriber communications over transports such as TCP/UDP/IP, and guarantees
compatibility among different DDS implementations.

eProsima Fast DDS 用于在标准网络上交换消息的协议是“实时”
发布-订阅协议(RTPS) <https://www.omg.org/spec/DDSI-RTPS/About-DDSI-RTPS/>`__，一种互操作线路
由OMG定义和维护的DDS协议
财团。
该协议通过传输协议(如TCP/UDP/IP)提供发布者-订阅者通信，并提供保证
不同DDS实现之间的兼容性。

Given its publish-subscribe roots and its specification designed for meeting the same requirements addressed by the DDS
application domain, the RTPS protocol maps to many DDS concepts and is therefore a natural choice for DDS
implementations.
All the RTPS core entities are associated with an RTPS domain, which represents an isolated communication plane where
endpoints match.
The entities specified in the RTPS protocol are in one-to-one correspondence with the DDS entities, thus allowing
the communication to occur.

考虑到它的发布-订阅根及其为满足DDS处理的相同需求而设计的规范
在应用程序领域，RTPS协议映射到许多DDS概念，因此是DDS的自然选择
实现。
所有RTPS核心实体都与一个RTPS域相关联，该域表示一个隔离的通信平面
端点匹配。
RTPS协议中指定的实体与DDS实体一一对应，因此允许
要发生的通信。

Main Features
^^^^^^^^^^^^^

* **Two API Layers.** *eProsima Fast DDS* comprises a high-level DDS compliant layer focused on usability and a
  lower-level RTPS compliant layer that provides finer access to the RTPS protocol.
  
  eProsima Fast DDS包括一个高级DDS兼容层，专注于可用性，另一个是底层RTPS兼容层，提供对RTPS协议更好的访问。

* **Real-Time behaviour.** *eProsima Fast DDS* can be configured to offer real-time features, guaranteeing responses
  within specified time constrains.

  eProsima Fast DDS可配置提供实时功能，保证响应在规定的时间内。

* **Built-in Discovery Server.** *eProsima Fast DDS* is based on the dynamical discovery of existing publishers and
  subscribers, and performs this task continuously without the need to contacting or setting any servers.
  However, a Client-Server discovery as well as other discovery paradigms can also be configured.

  eProsima Fast DDS基于现有发布者和订阅者的动态发现，并且不需要联系或设置任何服务器就可以连续执行此任务。
  但是，还可以配置Client-Server发现以及其他发现范式。


* **Sync and Async publication modes.** *eProsima Fast DDS* supports both synchronous and asynchronous data publication.
  
  eProsima Fast DDS支持同步和异步数据发布。

* **Best effort and reliable communication.** *eProsima Fast DDS* supports an optional reliable communication paradigm
  over *Best Effort* communications protocols
  such as UDP. Furthermore, another way of setting a reliable communication is to use our TCP transport.

  *eProsima Fast DDS*支持可选的可靠通信范例通过*尽最大努力*通信协议例如UDP。此外，另一种设置可靠通信的方法是使用我们的TCP传输。

* **Transport layers.** *eProsima Fast DDS* implements an architecture of pluggable transports. The current version
  implements five transports: UDPv4, UDPv6, TCPv4, TCPv6 and SHM (shared memory).

  *eProsima Fast DDS*实现了可插拔传输的架构。当前版本实现五种传输:UDPv4, UDPv6, TCPv4, TCPv6和SHM(共享内存)。

* **Security.** *eProsima Fast DDS* can be configured to provide secure communications. For this purpose, it implements
  pluggable security at three levels: authentication of remote participants, access control of entities and encryption
  of data.

  *eProsima快速DDS*可配置提供安全通信。为此目的，它实现了三个级别的可插拔安全性:远程参与者的身份验证、实体的访问控制和加密
的数据。

* :ref:`Statistics Module. <statistics>` *eProsima Fast DDS* can be configured to gather and provide information
  about the data being exchanged by the user application.

  *eProsima Fast DDS*可以配置为收集和提供信息关于用户应用程序正在交换的数据。

* **Flow controllers.** We support user-configurable flow controllers, that can be used to limit the amount
  of data to be sent under certain conditions.

  我们支持用户可配置的流量控制器，可用于限制流量在一定条件下发送的数据。

* **Plug-and-play Connectivity.** New applications and services are automatically discovered, and can join and leave
  the network at any time without the
  need for reconfiguration.

  新的应用程序和服务将被自动发现，可以在任何时候加入和离开网络，而不需要重新配置。

* **Scalability and Flexibility.** DDS builds on the concept of a global data space. The middleware is in charge of
  propagating the information between publishers and subscribers. This guarantees that the distributed network is
  adaptable to reconfigurations and scalable to a large number of entities.

  DDS构建在全局数据空间的概念之上。中间件负责在发布者和订阅者之间传播信息。这保证了分布式网络可适应重新配置，并可扩展到大量实体。

* **Application Portability.** The DDS specification includes a platform specific mapping to IDL, allowing an
  application using DDS to switch among DDS implementations with only a re-compile.

  DDS规范包括到IDL的特定于平台的映射，允许应用程序使用DDS在DDS实现之间切换，只需要重新编译。

* **Extensibility.** *eProsima Fast DDS* allows the protocol to be extended and enhanced with new services without
  breaking backwards compatibility and interoperability.

  *eProsima Fast DDS*允许协议扩展和增强新的服务没有打破向后兼容性和互操作性。

* **Configurability and Modularity.** *eProsima Fast DDS* provides an intuitive way to be configured, either through
  code or XML profiles. Modularity allows simple devices to implement a subset of the protocol and still participate in
  the network.

  *eProsima Fast DDS*提供了一种直观的配置方式代码或XML概要文件。模块化允许简单的设备实现协议的一个子集，并且仍然参与其中网络。

* **High performance.** *eProsima Fast DDS* uses a static low-level serialization library,
  `Fast CDR <https://github.com/eProsima/Fast-CDR>`__,
  a C++ library that serializes according to the standard CDR serialization mechanism defined in the `RTPS
  Specification <https://www.omg.org/spec/DDSI-RTPS/>`__ (see the Data Encapsulation chapter as a reference).

  *eProsima快速DDS*使用静态低级序列化库，'Fast CDR <https://github.com/eProsima/Fast-CDR> ' __，一个c++库，根据RTPS中定义的标准CDR序列化机制进行序列化规范<https://www.omg.org/spec/DDSI-RTPS/>`__(参考数据封装章节)。

* **Easy to use.** The project comes with an out-of-the-box example, the *DDSHelloWorld*
  (see :ref:`getting_started`) that puts into communication a
  publisher and a subscriber, showcasing how *eProsima Fast DDS* is deployed.
  Additionally, the interactive demo *ShapesDemo* is available for the user to dive into the DDS world.
  The DDS and the RTPS layers are thoroughly explained in the :ref:`DDS Layer <dds_layer>` and
  :ref:`RTPS Layer <rtps_layer>` sections.

  该项目附带了一个开箱即用的示例，*DDSHelloWorld*(参见:ref: ' getting_started ')，它会将a放入通信中发布者和订阅者，展示如何*eProsima快速DDS*部署。此外，交互式演示*ShapesDemo*可供用户深入到DDS世界。DDS和RTPS层在:ref: ' DDS Layer &lt;dds_layer&gt; '和中有详细的解释:ref: ' RTPS Layer &lt;rtps_layer&gt; ' section。

* **Low resources consumption.** *eProsima Fast DDS*:

  * Allows to preallocate resources, to minimize dynamic resource allocation. 允许预分配资源，最大限度地减少动态资源分配。
  * Avoids the use of unbounded resources. 避免使用无限资源
  * Minimizes the need to copy data. 尽量减少复制数据。

* **Multi-platform.** The OS dependencies are treated as pluggable modules.
  Users may easily implement platform modules using the *eProsima Fast DDS* library on their target platforms.
  By default, the project can run over Linux, Windows and MacOS.

  操作系统依赖项被视为可插拔模块。
  用户可以在目标平台上使用*eProsima Fast DDS*库轻松实现平台模块。
  默认情况下，该项目可以在Linux、Windows和MacOS上运行。

* **Free and Open Source.** The Fast DDS library, the underneath RTPS library, the generator tool, the internal
  dependencies (such as *eProsima Fast CDR*) and the external ones (such as the *foonathan* library) are free and
  open source.
  
  Fast DDS库，下面的RTPS库，生成器工具，内部依赖(如*eProsima Fast CDR*)和外部(如*foonathan*库)是免费的开源的。

Contacts and Commercial support
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Find more about us at `eProsima's webpage <https://eprosima.com/>`__.

Support available at:

* Email: support@eprosima.com
* Phone: +34 91 804 34 48

Contributing to the documentation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

*Fast DDS-Docs* is an open source project, and as such all contributions, both in the form of feedback and content
generation, are most welcomed.
To make such contributions, please refer to the
`Contribution Guidelines <https://github.com/eProsima/all-docs/blob/master/CONTRIBUTING.md>`_ hosted in our GitHub repository.

Fast DDS-Docs是一个开源项目，因此所有的贡献，无论是在反馈和内容的形式
一代，最受欢迎。如欲作出有关贡献，请参阅
`Contribution Guidelines <https://github.com/eProsima/all-docs/blob/master/CONTRIBUTING.md>`_托管在我们的GitHub存储库中。

中文翻译
^^^^^^^^

该文档由机器接口翻译而成，如果有错误的地方还请联系:

Email: 928559572@qq.com


Structure of the documentation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This documentation is organized into the sections below.

本文档分为以下几个部分:

* :ref:`Installation Manual <linux_binaries>`
* :ref:`Fast DDS <getting_started>`
* :ref:`Fast DDS-Gen <fastddsgen_intro>`
* :ref:`Release Notes <release_notes>`
