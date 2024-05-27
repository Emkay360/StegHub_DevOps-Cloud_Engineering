## What is the OSI Model?

The Open Systems Interconnection (OSI) model is a conceptual framework that divides network communications functions into seven layers. Sending data over a network is complex because various hardware and software technologies must work cohesively across geographical and political boundaries. The OSI data model provides a universal language for computer networking, so diverse technologies can communicate using standard protocols or rules of communication. Every technology in a specific layer must provide certain capabilities and perform particular functions to be useful in networking. Technologies in the higher layers benefit from abstraction as they can use lower-level technologies without worrying about underlying implementation details.

### Why is the OSI model important?

The layers of the Open Systems Interconnection (OSI) model encapsulate every type of network communication across both software and hardware components. The model was designed to allow two standalone systems to communicate via standardized interfaces or protocols based on the current layer of operation.

### The benefits of the OSI model 

__1. Shared understanding of complex systems__

Engineers can use the OSI model to organize and model complex networked system architectures. They can separate the operating layer of each system component according to its main functionality. The ability to decompose a system into smaller, manageable parts via abstraction makes it easier for people to conceptualize it as a whole.

__2. Faster research and development__

With the OSI reference model, engineers can understand their work better. They know which technological layer (or layers) they’re developing for when they create new, networked systems that need to communicate with each other. Engineers can develop networked systems and take advantage of a series of repeatable processes and protocols. 

__3. Flexible standardization__

The OSI model does not specify the protocols to use between levels, but rather the tasks that protocols perform. It standardizes network communication development so people can rapidly understand, build, and decompose highly complex systems—all  without prior knowledge of the system. It also abstracts details, so engineers don’t need to understand
every aspect of the model. In modern applications, the lower levels of networking and protocols are abstracted away to simplify system design and development. The following image shows how the OSI model is used in modern application development.

![OSI Model](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fd1fb711-7491-448f-8001-3b79162e058c)

## The Seven Layers of The OSI Model

The Open Systems Interconnection (OSI) model was developed by the International Organization for Standardization and others in the late 1970s. It was published in its first form in 1984 as ISO 7498, with the current version being ISO/IEC 7498-1:1994. 

__1. Physical layer__

The physical layer refers to the physical communication medium and the technologies to transmit data across that medium. At its core, data communication is the transfer of digital and electronic signals through various physical channels like fiber-optic cables, copper cabling, and air. The physical layer includes standards for technologies and metrics closely related to the channels, such as Bluetooth, NFC, and data transmission speeds.

__2. Data link layer__

The data link layer refers to the technologies used to connect two machines across a network where the physical layer already exists. It manages data frames, which are digital signals encapsulated into data packets. Flow control and error control of data are often key focuses of the data link layer. Ethernet is an example of a standard at this level. The data link layer is often split into two sub-layers: the Media Access Control (MAC) layer and Logical Link Control (LLC) layer. 

__3. Network layer__

The network layer is concerned with concepts such as routing, forwarding, and addressing across a dispersed network or multiple connected networks of nodes or machines. The network layer may also manage flow control. Across the internet, the Internet Protocol v4 (IPv4) and IPv6 are used as the main network layer protocols.

__4. Transport layer__

The primary focus of the transport layer is to ensure that data packets arrive in the right order, without losses or errors, or can be seamlessly recovered if required. Flow control, along with error control, is often a focus at the transport layer. At this layer, commonly used protocols include the Transmission Control Protocol (TCP), a near-lossless connection-based protocol, and the User Datagram Protocol (UDP), a lossy connectionless protocol. TCP is commonly used where all data must be intact (e.g. file share), whereas UDP is used when retaining all packets is less critical (e.g. video streaming).

__5. Session layer__

The session layer is responsible for network coordination between two separate applications in a session. A session manages the beginning and end of a one-to-one application connection and synchronization conflicts. Network File System (NFS) and Server Message Block (SMB) are commonly used protocols at the session layer.

__6. Presentation layer__

The presentation layer is primarily concerned with the syntax of the data itself for applications to send and consume. For example, Hypertext Markup Language (HTML), JavaScipt Object Notation (JSON), and Comma Separated Values (CSV) are all modeling languages to describe the structure of data at the presentation layer. 

__7. Application layer__

The application layer is concerned with the specific type of application itself and its standardized communication methods. For example, browsers can communicate using HyperText Transfer Protocol Secure (HTTPS), and HTTP and email clients can communicate using POP3 (Post Office Protocol version 3) and SMTP (Simple Mail Transfer Protocol).

Not all systems that use the OSI model implement every layer

## How does communication happen in the OSI model?

The layers in the Open Systems Interconnection (OSI) model are designed so that an application can communicate over a network with another application on a different device, no matter the complexity of the application and underlying systems. To do this, various standards and protocols are used to communicate with the layer above or below. Each of the layers is independent and only aware of the interfaces to communicate with the layer above and below it. 

By chaining together all these layers and protocols, complex data communications can be sent from one high-level application to another. The process works as follows:

1. The sender’s application layer passes data communication down to the next lower layer.
2. Each layer adds its headers and addresses to the data before passing it on. 
3. Data communication moves down the layers until it is eventually transmitted through the physical medium.
4. At the other end of the medium, each layer processes the data according to the relevant headers at that level. 
5. At the receiver end, data moves up the layer and is gradually unpacked until the application at the other end receives it.

### What are alternatives to the OSI model?

Various networking models were used in the past, such as Sequenced Packet Exchange/Internet Packet Exchange (SPX/IPX) and Network Basic Input Output System (NetBIOS). Today, the main alternative to the Open Systems Interconnection (OSI) model is the TCP/IP model.

__The TCP/IP model__

The TCP/IP model is comprised of five different layers:

- The physical layer
- The data link layer
- The network layer
- The transport layer
- The application layer
  
While layers like the physical layer, network layer, and application layer appear to map directly to the OSI model, this isn’t quite the case. Instead, the TCP/IP model most accurately maps to the structure and protocols of the internet.

In conclusion, the OSI model remains a popular networking model to describe how networking operates from a holistic perspective for educational purposes. However, the TCP/IP model is now more commonly used in practice.
