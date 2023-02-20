.. _what_is_dds:

What is DDS?
------------

The `Data Distribution Service (DDS) <https://www.omg.org/spec/DDS/About-DDS/>`_
is a data-centric communication protocol used for distributed software
application communications.
It describes the communications Application Programming Interfaces (APIs) and Communication Semantics that enable
communication between data providers and data consumers.

数据分发服务(DDS &lt;https://www.omg.org/spec/DDS/About-DDS/&gt;) _
分布式软件是否使用以数据为中心的通信协议
应用程序通信。
它描述了启用的通信应用程序编程接口(api)和通信语义
数据提供者和数据使用者之间的通信。

Since it is a Data-Centric Publish Subscribe (DCPS) model, three key application entities are defined in its
implementation: publication entities, which define the information-generating objects and their properties;
subscription entities, which define the information-consuming objects and their properties; and configuration entities
that define the types of information that are transmitted as topics, and create the publisher and subscriber with
its Quality of Service (QoS) properties, ensuring the correct performance of the above entities.

由于它是一个以数据为中心的发布订阅(dps)模型，因此在它中定义了三个关键的应用程序实体
实现:发布实体，定义信息生成对象及其属性;
订阅实体，定义了使用信息的对象及其属性;和配置实体
定义作为主题传输的信息类型，并使用
其服务质量(QoS)属性，确保上述实体的正确性能。

DDS uses QoS to define the behavioral characteristics of DDS Entities. QoS are comprised of individual QoS policies
(objects of type deriving from QoSPolicy). These are described in :ref:`dds_layer_core_policy`.


DDS使用QoS定义DDS实体的行为特征。QoS由各个QoS策略组成
(派生自QoSPolicy类型的对象)。这些描述在:ref: ' dds_layer_core_policy '中。

The DCPS conceptual model
^^^^^^^^^^^^^^^^^^^^^^^^^

In the DCPS model, four basic elements are defined for the development of a system of communicating applications.

*   **Publisher**.
    It is the DCPS entity in charge of the creation and configuration of the **DataWriters** it implements.
    The **DataWriter** is the entity in charge of the actual publication of the messages.
    Each one will have an assigned **Topic** under which the messages are published.
    See :ref:`dds_layer_publisher` for further details.

    它是DCPS实体，负责创建和配置它实现的 **datawriter** , **DataWriter** 是负责实际发布消息的实体。每一个都有一个指定的 **Topic**，在该Topic下发布消息。详见:ref:`dds_layer_publisher`。

*   **Subscriber**.
    It is the DCPS Entity in charge of receiving the data published under the topics to which it subscribes.
    It serves one or more **DataReader** objects, which are responsible for communicating the availability of new data
    to the application.
    See :ref:`dds_layer_subscriber` for further details.

    DCPS实体负责接收在其订阅的主题下发布的数据。
    它为一个或多个 **DataReader** 对象提供服务，这些对象负责传递新数据的可用性
    到应用程序。详情见:ref:`dds_layer_subscriber`。

*   **Topic**.
    It is the entity that binds publications and subscriptions.
    It is unique within a DDS domain.
    Through the **TopicDescription**, it allows the uniformity of data types of publications and subscriptions.
    See :ref:`dds_layer_topic` for further details.
    
    它是绑定发布和订阅的实体。
    它在DDS域中是唯一的。
    通过 **TopicDescription** ，允许发布和订阅数据类型的一致性。
    详情见:ref:`dds_layer_topic`。

*   **Domain**.
    This is the concept used to link all publishers and subscribers, belonging to one or more applications,
    which exchange data under different topics.
    These individual applications that participate in a domain are called **DomainParticipant**.
    The DDS Domain is identified by a domain ID.

    这一概念用于连接属于一个或多个应用程序的所有发布者和订阅者，
    在不同的主题下交换数据。
    这些参与域的独立应用程序被称为 **DomainParticipant**。
    DDS域由Domain ID标识。

    The DomainParticipant defines the domain ID to specify the DDS domain to which it belongs.
    Two DomainParticipants with different IDs are not aware of each other's presence in the network.
    Hence, several communication channels can be created.

    DomainParticipant通过定义domain ID来指定所属的DDS域。
    具有不同id的两个domainparticipant不知道彼此在网络中的存在。
    因此，可以创建多个通信通道。

    This is applied in scenarios where several DDS applications are involved, with their respective DomainParticipants
    communicating with each other, but these applications must not interfere.
    The **DomainParticipant** acts as a container for other DCPS Entities, acts as a factory for
    **Publisher**, **Subscriber** and **Topic** Entities, and provides administrative services in the domain.
    See :ref:`dds_layer_domain` for further details.

    这适用于涉及多个DDS应用程序的场景，以及它们各自的DomainParticipants
    相互通信，但这些应用程序不能相互干扰。
    DomainParticipant充当了其他dps实体的容器，充当了dps实体的工厂
    **Publisher**， **Subscriber**和 **Topic** 实体，在域中提供管理服务。
    See :ref:`dds_layer_domain`。



These elements are shown in the figure below.

.. figure:: /01-figures/fast_dds/getting_started/dds_domain.svg
    :align: center

    DCPS model entities in the DDS Domain.



