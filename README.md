# The developers guide to arrowhead

## Introduction
Arrowhead Framework uses a Service-Oriented Architecture (SOA) to achieve an efficient IoT automation.
It helps you with the integration of different devices and protocols in your production, allows you to easily log and monitor important variables, and assures your production and communication operates in a secure manner. Arrowhead Framework allows you to *spend most of your time focusing on achieving your higher-level goals such as workflow optimization, quality assurance, custom production, and more*.

## Table of contents

## The Local Cloud and other Arrowhead entities
As per the Arrowhead approach, a [Service](definitions/service.md) Oriented Architecture (SOA) is established in order to provide interoperability by facilitating the service interactions within closed or at least separated automation environments, called local clouds.

Arrowhead Local Clouds can fulfill various tasks, and can have their own sets of appointed stakeholders (e.g. their operators or developers). These Local Clouds all have their operational boundaries, let those be functional, geographical or network-segmented. Nevertheless, each local cloud is governed through their own instances of the Arrowhead Core Systems. These are clouds in the sense that they use common resources: the Core Systems of that domain. In Arrowhead Local Clouds there can be an arbitrary number of Systems that can provide and consume Services from one another: they create and finish servicing instances dynamically in run-time.

In the Arrowhead framework, the following abbreviations are used:  

* Arrowhead Local Cloud, Local Cloud: a group of (not just computational) resource bounded together to achieve a higher-level purpose and functionality (in accordance with the System-of-Systems and automation clouds concepts)
* Service Consumer, Consumer: The System that requests a resource from another System (initiates the communication and requests orchestration)
* Service Provider, Provider, Producer: The System that will serve the request from the Consumer. 
* Service offering: when Service Providers implement a certain Service, they announce their capabilities (in the Service Registry), hence their Service offerings.
* Servicing instance: when two specific System instances provide and consume a specific Service from one another, this transitional connection is called a servicing instance (basically, when a Service is realized). 

In this context, Arrowhead-compatible Systems are not just service endpoints, but can be realized by a wide range of devices: from a small temperature sensor, in certain use cases, up to very complex cyber-physical systems of a production plant. However, there might be operational limits of how these Systems can be handled, i.e. certain hard-wired service interactions cannot change at all, supposing that an actuator can only access certain sensors. The Arrowhead Framework has to abide by such implementational constraints that shape this world of the industrial Internet of Things.


## Core services
The Arrowhead framework consists of a set of core systems that exist in each Arrowhead local cloud. We will begin to consider these services from a higher abstraction level and then go through each system in more detail. Starting from the five Ws we get a great view on the core systems:  

* **Who** - *Who can be part of a local cloud and who can use the different services?* This question is answered by the **Authorization** core system.
* **What** - *What services exist and how do I connect to them?* The **Orchestrator** core system allows one to search for services both within the local cloud but also in other connected local clouds. 
* **When** - *How do I schedule important events?* In order to facilitate the periodic scheduling, and to signal important events across services you can use the **EventHandler** core system.
* **Where** - *Where do I connect to other local clouds?* In order to interact with other local clouds in an easy and secure fashion, you should use the **GateKeeper** core service.
* **Why** - *Why did that happen?* The **Logger** core service can help you by providing important information and status about your local cloud that is useful when trying to find problems.

The picture below shows the different core systems.  
**PICTURE OF CORE SYSTEMS HERE...**


##  Authorization
The purpose of the Authorization System is to:  

* Provide AuthorizationControl Service (both intra- and inter-Cloud)
* Provide a TokenGeneration Service for allowing session control within the Local Cloud

This core system holds a database that describes which Application System that are allowed to consume certain services in the local cloud (intra-Cloud access rules), but also which other Local Clouds that  are allowed to consume Services from this local cloud (inter-Cloud authorization rules). Examples of how this access is granted is provided below.

**PICTURE OF INTRA-CLOUD AUTH TABLE/RELATIONS HERE...**

The Authorization system is also in charge of the authentication of Systems in the local cloud. Authentication is achieved using X.509 certificates, and the Authorization system stores all X.509 certificate PublicKeys for every System in the Cloud. Authentication between Systems in the local cloud is facilitated by having the systems  passing session tokens between them that they have received from the Authorization System. Each system can then verify the identity of another party by validating the token using the to-be-authenticated party's public key. An example of this authentication and validation procedure is found below.

**PICTURE OF TOKEN GENERATION, PASSING AND VALIDATION HERE...**


### Produced services
The Authorization System offers two Core Services:  

* AuthorizationControl <-- **MAKE LINK TO AUTHIRAZTION SERVICES HERE...**
* TokenGeneration <-- **MAKE LINK TO AUTHIRAZTION SERVICES HERE...**

The purpose of the *TokenGeneration* functionality is to create session control functionality through the Core Sytems. The output is an ArrowheadToken that validates the Service Consumer system when it will try to access the Service from another Application System (Service Provider). This Token shall be primarily generated during the orchestration process and only released to the Service Consumer when all affected Core Systems are notified and agreed to the to-be-established Service connection. 

This System (in line with all core Systems) utilizes the X.509 certificate Common Name naming convention in order to work. This means that the CN is structured as it is described in the generic G4.0 System-of-System Design Document (SoSDD). 
