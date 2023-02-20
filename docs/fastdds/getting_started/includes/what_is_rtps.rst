.. _what_is_rtps:

What is RTPS?
-------------

The `Real-Time Publish Subscribe (RTPS) <https://www.omg.org/spec/DDSI-RTPS/2.2/PDF>`_ protocol, developed to
support DDS applications, is a publication-subscription communication middleware
over best-effort transports such as UDP/IP. Furthermore, Fast DDS provides support for TCP and
Shared Memory (SHM) transports.

实时发布订阅(RTPS &lt;https://www.omg.org/spec/DDSI-RTPS/2.2/PDF&gt;) _协议，开发用于
支持DDS应用程序，是一种发布-订阅通信中间件
优于UDP/IP等尽力传输。此外，Fast DDS提供了对TCP和
共享内存(SHM)传输。

It is designed to support both unicast and multicast communications.

它被设计成同时支持单播和多播通信。

At the top of RTPS, inherited from DDS, the **Domain** can be found, which defines a separate plane of communication.
Several domains can coexist at the same time independently.
A domain contains any number of **RTPSParticipants**, that is, elements capable of sending and receiving data.
To do this, the RTPSParticipants use their **Endpoints**:

在RTPS的顶部，继承自DDS，可以找到 **Domain**，它定义了一个单独的通信平面。
多个域可以同时独立共存。
一个域包含任意数量的 **RTPSParticipants**，即能够发送和接收数据的元素。
为此，RTPSParticipants使用他们的 **Endpoints**:

 

* **RTPSWriter**: Endpoint able to send data.
* **RTPSReader**: Endpoint able to receive data.

A RTPSParticipant can have any number of writer and reader endpoints.

RTPSParticipant可以有任意数量的写入器和读取器端点。

.. figure:: /01-figures/fast_dds/getting_started/rtps_domain.svg
    :scale: 100 %
    :align: center

    RTPS high-level architecture

Communication revolves around **Topics**, which define and label the data being exchanged.
The topics do not belong to a specific participant.
The participant, through the RTPSWriters, makes changes in the data published under a topic, and through the RTPSReaders
receives the data associated with the topics to which it subscribes.

通信围绕 **Topics**展开，这些主题定义并标记了正在交换的数据。
主题不属于特定的参与者。
参与者通过rtpswriter对主题下发布的数据进行更改，并通过RTPSReaders进行更改
接收与其订阅的主题相关联的数据。

The communication unit is called **Change**, which represents an update in the data that is written under a Topic.
**RTPSReaders/RTPSWriters** register these changes on their **History**, a data structure that serves as a cache for
recent changes.

通信单元称为 **Change**，它表示在Topic下写入的数据中的更新。
**RTPSReaders/ RTPSWriters **在它们的 **History**上注册这些更改，这是一个数据结构，用于缓存
最近的变化。

In the default configuration of *eProsima Fast DDS*, when you publish a `change` through a RTPSWriter endpoint, the
following steps happen behind the scenes:

在 *eProsima Fast DDS*的默认配置中，当您通过RTPSWriter端点发布“更改”时，会将
发生一下步骤:

1. The `change` is added to the RTPSWriter’s history cache.    //“Change”被添加到RTPSWriter的历史缓存中  
2. The RTPSWriter sends the change to any RTPSReaders it knows about.  //RTPSWriter将更改发送给它所知道的所有RTPSReaders
3. After receiving data, RTPSReaders update their history cache with the new change.  //接收到数据后，RTPSReaders使用新的"change"更新其历史缓存。

However, Fast DDS supports numerous configurations that allow you to change the behavior of RTPSWriters/RTPSReaders.
A modification in the default configuration of the RTPS entities implies a change in the data exchange flow between
RTPSWriters and RTPSReaders.
Moreover, by choosing Quality of Service (QoS) policies, you can affect how these history caches are managed in several
ways, but the communication loop remains the same.
You can continue reading section :ref:`rtps_layer` to learn more about the implementation of the RTPS protocol in Fast
DDS.

但是，Fast DDS支持许多配置，允许您更改rtpswriter / rtpsreader的行为。
RTPS实体的默认配置的修改意味着之间的数据交换流的更改
rtpswriter和rtpsreader。
此外，通过选择服务质量(QoS)策略，可以在多个策略中影响这些历史缓存的管理方式
方式，但沟通循环保持不变。
您可以继续阅读部分:ref:`rtps_layer` 以了解更多关于Fast DDS中RTPS协议的实现
。



