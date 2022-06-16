# Actors and Components

The Ouinet network relies on the cooperation of a few different parties with different roles and responsibilities. Some of these components form a centralized infrastructure, whereas others form a decentralized cooperative of users and contributors to the system.


## The injector servers

The main backing infrastructure of the Ouinet network consists of a set of *injector servers*. A Ouinet injector server is a variant of an open proxy server, that users can connect to in order to gain access to web resources they cannot access via more direct means. Injector servers also have other responsibilities, described in more detail in the [Distributed Cache](#distributed-cache) section.

Injector servers form a trusted part of the Ouinet network. In their role as an intermediary between a user and their communication to much of the internet, they are in a position where a malicious implementation could do serious harm to user security. To keep track of the impact of this trusted position, injector servers are identified by a cryptographic identity that is used to authenticate the injector server when connecting to it, as well as certifying data published by the injector server.

Different organizations can run their own collections of injector servers, and users of the Ouinet system can configure their devices or applications to use whatever injector servers they wish to trust. The Ouinet organization operates one such cluster of injector servers for general purpose use, but there is no requirement for anyone using the Ouinet system to make use of these injector servers, and grant it authority thereby.


## The client library

On the user end, the Ouinet project provides a piece of software that user applications can use to access web resources through the Ouinet network, referred to as the *client library*. As the name suggests, it is implemented as a library to be used by end-user applications that make use of access to the web, rather than being an application in its own right. The detailed behavior of the client library is described in the [Client Library](#client-library) section.

In addition to the logic necessary to access web resources through the different systems that Ouinet provides for this purpose, the client library also contains the functionality required for the application to participate in the peer-to-peer components of the Ouinet network. Active participation in these peer-to-peer systems is not a requirement when using the client library, and indeed there exist applications for which active involvement in the network is unlikely to work well. The aim of this integration is to allow an application to enable contribution to the network where this is reasonably practical, and forego it otherwise.


## Support nodes

The Ouinet network relies on the existence of a substantial community of users that contribute to the peer-to-peer systems that underlie Ouinet. However, it is to be expected that many users of the Ouinet library will do so on low-powered devices for which a significant peer-to-peer contribution is not very practical.

To remedy this, users with access to well-connected devices can contribute to the resources of the Ouinet network by operating a *support node*. This is a server that runs a configuration of the client library, implemented as a standalone program, which does not serve any immediate applications for its operator, but instead is setup solely to contribute its resources to the Ouinet peer-to-peer systems. The presence of a few strategically-placed volunteer support nodes can greatly benefit the performance and reliability of the Ouinet network.


## End-user applications

The Ouinet project proper does not contain any end-user applications that make use of the Ouinet client library; the client library is designed as a building block for others to build upon, rather than as a tool that is useful for end users directly. But of course, the Ouinet project relies very much on the existence, quality, and useful application of such external projects.
