# Cloud Director Extension Framework

Cloud Director aims to deliver open and extensible platform that allows providers to introduce new functionality into their deployment and enrich the set of services offered. Following is an overview of the technologies used to achieve this goal.
## Message bus

The backbone of many of the extensibility framework technologies is a message bus. Extension backends communicate with-, or monitor various system processes and events. Currently, there are two message busses - one for the AMQP protocol, which is backed by a provider's own RabbitMQ server, and one for the MQTT protocol which is embedded in Cloud Director. AMQP is being slowly phased out in favour of MQTT and will eventually be completely removed.
## Extension Services

The Cloud Director API includes a framework for integration of extension services, which REST API clients can access as though they were native services. In addition to service-specific objects or operations they provide, extension services can implement new operations for existing API objects.

A Cloud Director extension service is a program that presents a REST interface to Cloud Director API clients. When you register an extension service with the Cloud Director API, you specify one or more URL patterns that the Cloud Director REST service treats as extension requests. There are two flavours of extension services - AMQP(legacy) and MQTT. 
## Object Extensions

Cloud Director object extensions are external applications that can participate in, influence, or override the logic that Cloud Director applies to workflows like vApp instantiation and placement.

An object extension can participate as a peer of the system in the logic that determines the outcome of these workflows. For example, an object extension might use information provided by the system to place a VM on a specific host or assign it a specific storage profile. Operation of object extensions is transparent to system users. An extension can also allow the Cloud Director procedures in which it participates to run to completion unmodified.
## Blocking Tasks(Callouts)

Most built-in long-running operations in Cloud Director can be configured to block and wait to be resumed/cancelled/aborted via an external entity - either manually by a user, or by an application. 
## Events

Most built-in operations produce audit events which are posted on the message bus, allowing external applications to react and create unique behaviour.
## UI Plugins

The built-in Cloud Director UI application provides various extension-, or mount points where providers can attach custom menus, pages, actions or elements. 
## Defined Entities

Defined entities represent external resources that Cloud Director can manage. Extension and UI plug-in developers can create runtime defined entities, enabling extensions and UI plug-ins to store and manipulate the extension-specific information in Cloud Director. If you create an extension, and it needs to store a state or references to external resources, the extension can use runtime defined entities instead of a local database. For example, a Kubernetes extension can store information about the Kubernetes clusters it manages in runtime defined entities. The extension can then provide extension APIs for managing those clusters using the information from the runtime defined entities.

You can access runtime defined entities only through the Cloud Director API. Users with admin privileges can track and observe the way extensions operate by verifying the state of the defined entities that the extensions create. The states of the defined entities can also help you to troubleshoot problems with the extensions.
## Object Metadata

Many core entity types support a notion of a soft schema extension to their main properties. Object metadata gives cloud operators and tenants a flexible way to associate user-defined properties (name=value pairs) with objects.

There are two separate pools of metadata, which have different management and capabilities:
1. Metadata of objects managed by the legacy Cloud Director api
2. Metadata of objects managed by the Cloud Director `cloudapi`

Objects which can be managed by both kinds of apis(legacy and cloudapi) will have the two separate pools. Metadata of the legacy api is considered deprecated and will receive only bug-fixes. Metadata of the cloudapi provides new features, with the primary goal being - improved _performance_, including a more powerful _search-by-metadata_ mechanism.
