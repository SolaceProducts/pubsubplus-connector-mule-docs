# Solace PubSub+ Connector for Mule 4

[_Certified_](https://www.mulesoft.com/legal/versioning-back-support-policy#anypoint-connectors)

Contents:
  * [Overview](#overview)
  * [Prerequisites](#prerequisites)
    + [Compatibility](#compatibility)
    + [Key Concepts](#key-concepts)
    + [Getting a Solace PubSub+ Messaging Service](#getting-a-solace-pubsub-messaging-service)
    + [Setting Up and Getting Access to the PubSub+ Event Catalog](#setting-up-and-getting-access-to-the-pubsub-event-catalog)
  * [Configurations](#configurations)
    + [General Configuration, Connection Tab](#general-configuration-connection-tab)
        * [PubSub+ broker Connection Details](#pubsub-broker-connection-details)
        * [JCSMP Properties](#jcsmp-properties)
    + [General Configuration, Security Tab](#general-configuration-security-tab)
    + [General Configuration, Advanced Tab](#general-configuration-advanced-tab)
        * [Maximum wait time for MANUAL_CLIENT Ack](#maximum-wait-time-for-manual_client-ack)
    + [Connector Defaults Configuration](#connector-defaults-configuration)
        * [Queue Access Type for Provisioned Queues](#queue-access-type-for-provisioned-queues)
        * [Encoding](#encoding)
        * [Content Type](#content-type)
    + [Event Portal Configuration](#event-portal-configuration)
        * [Event Portal API Token](#event-portal-api-token)
        * [Event Portal Console Organization Prefix](#event-portal-console-organization-prefix)
  * [Accelerate Application Development Using the Event Catalog](#accelerate-application-development-using-the-event-catalog)
  * [Common Parameters](#common-parameters)
      - [Event Portal integration related Parameters](#event-portal-integration-related-parameters)
          + [Event Portal Event](#event-portal-event)
      - [Provisioning Parameters](#provisioning-parameters)
          + [Provision Option](#provision-option)
      - [Content Type and Encoding Parameters](#content-type-and-encoding-parameters)
          + [Content Type and Encoding](#content-type-and-encoding)
  * [Common Attributes for Inbound Messages](#common-attributes-for-inbound-messages)
  * [Operations](#operations)
    + [Publish Operation](#publish-operation)
      - [Required Parameters](#required-parameters)
        * [Connector Configuration](#connector-configuration)
        * [Destination](#destination)
      - [Optional Parameters](#optional-parameters)
          + [Message](#message)
      - [Example](#example)
    + [Consume Operation](#consume-operation)
      - [Required Parameters](#required-parameters-1)
        * [Connector Configuration](#connector-configuration-1)
        * [Endpoint](#endpoint)
      - [Optional Parameters](#optional-parameters-1)
          + [Time Out](#time-out)
      - [Example](#example-1)
    + [Ack Operation](#ack-operation)
      - [Required Parameters](#required-parameters-2)
        * [Connector Configuration](#connector-configuration-2)
        * [Message Reference Id](#message-reference-id)
      - [Example](#example-2)
    + [Request-Reply Operation](#request-reply-operation)
      - [Required Parameters](#required-parameters-3)
        * [Connector Configuration](#connector-configuration-3)
        * [Request Destination](#request-destination)
      - [Optional Parameters](#optional-parameters-2)
          + [Request Message](#request-message)
          + [Time Out](#time-out-1)
      - [Example](#example-3)
  * [Sources](#sources)
    + [Direct Topic Subscriber source](#direct-topic-subscriber-source)
      - [Required Parameters](#required-parameters-4)
        * [Connector Configuration](#connector-configuration-4)
        * [Subscriptions](#subscriptions)
      - [Optional Parameters](#optional-parameters-3)
      - [Example](#example-4)
    + [Guaranteed Endpoint Listener source](#guaranteed-endpoint-listener-source)
      - [Required Parameters](#required-parameters-5)
        * [Connector Configuration](#connector-configuration-5)
        * [Endpoint](#endpoint-1)
      - [Optional Parameters](#optional-parameters-4)
      - [Example](#example-5)
  * [Error Handling](#error-handling)

</br>

## Overview

The Solace PubSub+ Connector has been implemented using the Solace PubSub+ high performance JCSMP API.

The connector exposes the following functionality to MuleSoft users and applications:

**Design-time**

*	PubSub+ Event Portal: integration with the Event Catalog in PubSub+ Event Portal
*	Automatic Queue Provisioning: option to automatically provision Solace queues on test PubSub+ event brokers and then add topic subscriptions to those queues (specified at desgin-time but actually created at runtime)

**Sources**

*	Direct Topic Subscriber: triggered when a direct message is received on a set of topic subscriptions
*	Guaranteed Endpoint Listener: triggered when a guaranteed message is received on a queue

**Operations**

* Publish: publishes a direct message to a topic or a persistent message to a queue
* Consume: consumes a single guaranteed message from an endpoint (Solace queue)
* Request-Reply: a blocking operation that sends a message to a topic or queue (configurable) and waits for a response on an automatically created temporary topic or queue
* Ack: acknowledges receipt of a guaranteed message
* Recover Session: perform session recover when consuming an unacknowledged Message

</br>

## Prerequisites

### Compatibility

| Application/Service | Version |
|---|---|
| Mule Runtime | 4.3 and higher |
| Studio Version | 7.9 and higher |
| PubSub+ Event Broker | 9.1 and higher |
| Java | 1.8 and later |

### Key Concepts

This document assumes that you are familiar with the MuleSoft Anypoint Platform (including Anypoint Connectors and Anypoint Studio), Mule concepts, elements in a Mule flow, and Global Elements. To get started, review the general [Anypoint Connector Configuration](https://docs.mulesoft.com/connectors/introduction/intro-connector-configuration-overview) section of the MuleSoft documentation.

This document also assumes that you are familiar with [Solace Core Concepts](https://docs.solace.com/PubSub-Basics/Core-Concepts.htm) and [Solace PubSub+ Cloud](https://docs.solace.com/What-Is-PubSub-Cloud.htm). In particular, we recommend that you review following sections from the Solace documentation:
* [Message Exchange Patterns](https://docs.solace.com/PubSub-Basics/Core-Concepts-Message-Models.htm) 
* [Message Delivery Modes](https://docs.solace.com/PubSub-Basics/Core-Concepts-Message-Delivery-Modes.htm)
* [Topics](https://docs.solace.com/PubSub-Basics/Understanding-Topics.htm) 
* [Endpoints and Queues](https://docs.solace.com/PubSub-Basics/Core-Concepts-Endpoints-Queues.htm), including the _Topic-to-Queue Mapping_ section

For more information about Solace technology in general please visit these resources:

- The [Solace Developer Portal](https://solace.dev)
- Ask the [Solace community](https://solace.community/)

### Getting a Solace PubSub+ Messaging Service

If you don't already have a Solace PubSub+ Event Broker available, use one of the options listed in the [Try PubSub+ Event Broker](https://docs.solace.com/PubSub-Basics/Getting-Started-Try-Broker.htm) section of the Solace documentation to quickly spin up a free Solace messaging service. To start using the connector, you'll need to supply the following connection properties for your broker (refer to the setup guide of your broker for information on how to obtain them):

| Property | Description |
|---|---|
| Client Username | Username to connect to the event broker |
| Client Password | Password to connect to the event broker |
| Message VPN | Specifies the Message VPN to use when connecting to the event broker |
| Broker Host | This is the address clients use when connecting to the event broker to send and receive messages. (Format: `tcps://FQDN:Port` or `tcps://IP:Port`, - insecure `tcp://` is also accepted) |

### Setting Up and Getting Access to the PubSub+ Event Catalog

You will need a [PubSub+ Cloud account](https://docs.solace.com/Solace-Cloud/gf_faqs.htm) to use design-time integration with PubSub+ Event Portal's Event Catalog. If you opted to use a PubSub+ Cloud Event Broker, use the same account. Otherwise [create an account](https://solace.com/products/platform/cloud/).

Review the [Event Catalog](https://docs.solace.com/Solace-Cloud/Event-Portal/event-portal-catalog.htm) documentation. To get going easily, you can [Import a Sample Architecture](https://docs.solace.com/Solace-Cloud/Event-Portal/get-started-event-portal-designer.htm#Import) into your portal if you don't already have one.

Once you have the Event Model with a catalog of events, try to [Browse Event Streams and Visualize their Relationships](https://docs.solace.com/Solace-Cloud/Event-Portal/get-started-event-portal-catalog.htm).

To access your Event Model from Anypoint Studio, follow the steps to [create an API Token](https://docs.solace.com/Solace-Cloud/ght_api_tokens.htm#Create), and select the following permission:
* Event Portal / Event Portal Read: Get objects in the event portal

![alt text](/doc/images/EPRequiredPermission.png "Required EP Token permission in Event Portal")

To use the token from your developer machine, define the following environment variable, replacing `<your-token>` with your generated API Token. Adjust the syntax to your OS environment, and also make this a permanent environment setting on your machine, so there is no need to distribute the token.
```
export SOLACE_EVENTPORTAL_API_TOKEN=<your-token>
```
> Note: providing the token through an environment variable to Anypoint Studio prevents you from distributing your personal token through Mule project files. Also note that access to PubSub+ Event Catalog is not required for run time, only for design time.

</br>
</br>

## Configurations

For a new PubSub+ connector configuration, create a new 'Solace PubSub+ Connector Config' Global Element.

### General Configuration, Connection Tab

Use the General Connection configuration tab to configure the connection to the PubSub+ event broker.

![alt text](/doc/images/NowConnectorConfig-GeneralConnection.png "New Connector Config - General Connection")

##### PubSub+ Broker Connection Details

Provide the Client Username, Client Password, Message VPN, and Broker Host parameters. For details, refer to the [Getting a Solace PubSub+ Messaging Service](#getting-a-solace-pubsub-messaging-service) section.

##### JCSMP Properties

Your use case may require you to set advanced properties of the Solace Java API (JCSMP). To do this, select the `JCSMP Properties` checkbox and provide the necessary key-value pairs. Assign a value to `<Property>` where `<Property>` is the name of the property as defined in the Solace JCSMP API documentation for [`com.solacesystems.jcsmp.JCSMPProperties`](https://docs.solace.com/API-Developer-Online-Ref-Documentation/java/constant-values.html#com.solacesystems.jcsmp.JCSMPProperties.ACK_EVENT_MODE). Tip: use the name in all capitals; in the example below, the `SUB_ACK_WINDOW_SIZE` property has been set to `100`.
> Note: to set a JCSMP Client Channel Property, prefix the [client channel property name](https://docs.solace.com/API-Developer-Online-Ref-Documentation/java/com/solacesystems/jcsmp/JCSMPChannelProperties.html) with `CLIENT_CHANNEL_PROPERTIES`, as in this example:

![alt text](/doc/images/NowConnectorConfig-GeneralConnection-JCSMPProp.png "New Connector Config: General Connection, JCSMP property")

An example XML representation of this configuration is as follows:

```xml
<solace:config name="Solace_PubSub__Connector_Config">
  <solace:connection msgVPN="myVPN" brokerHost="tcps://myEventBroker:55443" clientUserName="myUsername" password="myPassword">
    <solace:jcsmp-properties >
      <solace:jcsmp-property key="SUB_ACK_WINDOW_SIZE" value="100" />
      <solace:jcsmp-property key="CLIENT_CHANNEL_PROPERTIES.connectRetries" value="5" />
    </solace:jcsmp-properties>
  </solace:connection>
</solace:config>
```

### General Configuration, Security Tab

Use the Security configuration tab to configure TLS options for the broker connection.

![alt text](/doc/images/NowConnectorConfig-Security.png "New Connector Config: Security")

For configuration details, refer to the [Configure TLS with Keystores and Truststores](https://docs.mulesoft.com/mule-runtime/latest/tls-configuration) section of the MuleSoft documentation.

> Note: setting trusted root certificates for a TLS connection is not required if they are already included in the default trusted public certificates of your local Java installation. This is likely the case for PubSub+ Cloud. However, you should verify whether this is the case in all target environments.

### General Configuration, Advanced Tab

Use the Advanced tab to configure additional options for the connector.

![alt text](/doc/images/NowConnectorConfig-Advanced.png "New Connector Config: Advanced")

#### Maximum wait time for MANUAL_CLIENT Ack

Select the checkbox to override the default configuration of "Maximum wait time for MANUAL_CLIENT Ack".

This is the maximum time allowed to acknowledge receipt of a guaranteed message by the application (MANUAL_CLIENT). Adjust this value if your expected processing time is higher.

> Note:  this setting doesn't apply to AUTOMATIC_ON_FLOW_COMPLETION Ack, where there is no such time limit.

### Connector Defaults Configuration

Use Connector Defaults to configure default settings for the connector.

![alt text](/doc/images/NowConnectorConfig-ConnectorDefaults.png "New Connector Config: Connector defaults")

#### Queue Access Type for Provisioned Queues

Adjust the "Queue Access Type for provisioned queues" to define the required [access type configuration](https://docs.solace.com/Configuring-and-Managing/Configuring-Queues.htm#Configur12) for queues, when provisioning is enabled.

#### Encoding

Set the default message encoding for the connector. This value is applied if the encoding is not provided in the Solace message or explicitly in either the Source or the Operation. If left blank, the Mule Runtime default encoding is used as the default for the connector.

#### Content Type

Set the default content type for the connector. This value is applied if the content type is not provided in the Solace message or explicitly in either the Source or the Operation.

### Event Portal Configuration

This configuartion can be used to configure access to the Solace PubSub+ Event Portal for design-time integration.

> **Important**: this configuration is provided for quick-start/experimental purposes only. Using this configuration, the app developer's personal token is saved in the project XML in clear text, and may get inadvertently distributed. We recommend that you provide this same configuration through local environment variables instead, before starting Anypoint Studio. 

</br>

![alt text](/doc/images/NowConnectorConfig-EventPortal.png "New Connector Config: Event Portal")

#### Event Portal API Token

For details about how to get the API token, refer to the [Setting Up and Getting Access to the PubSub+ Event Catalog](#setting-up-and-getting-access-to-the-pubsub-event-catalog) section.

Although you can use this field to configure access to Event Portal, it results in the token being written to the Mule project XML file, and therefore should be used for quick-start or experimental purposes only. We recommend instead that you set the global `SOLACE_EVENTPORTAL_API_TOKEN` environment variable to the EP Token value to achieve the same effect.
```XML
<solace:config name="Solace_PubSub_Connector_Config">
  <solace:event-portal-config cloudApiToken="myToken" />
</solace:config>
```

#### Event Portal Console Organization Prefix

This parameter should be set for non-default organizations and is used to determine the HTTP link displayed for the selected event in Event Portal. If not provided, the default value is used (`console.solace.cloud`).
> For example: A value of `mycompany-sso` would result in Event Portal links like: `https://mycompany-sso.solace.cloud/...`

Although this is not a secret setting, if your org prefix is different than the default we recommend that you set the global `SOLACE_EVENTPORTAL_CONSOLE_ORG_PREFIX` environment variable, similar to the mechanism used to set the API Token.

</br>

## Accelerate Application Development Using the Event Catalog

Using Solace PubSub+ Event Portal integration speeds up development by allowing you to fetch event (message) details from a central event database, the Event Catalog.

If you have [enabled and configured](#setting-up-and-getting-access-to-the-pubsub-event-catalog) integration with PubSub+ Event Catalog, you can use PubSub+ Mule Connector's source and operation components (except for the Ack operation) to select from a list of available* events from the connected Catalog, and use that event as a model for the message in the component. A number of the component's configuration parameter fields are auto-populated including the topic name associated with the event, and a suggested generated queue name—both can be used as the messaging destinations. For developers there is also an option to automatically create the suggested queue on the PubSub+ Event Broker. Additionally, the JSON or XML schema of the message is also fetched from the Catalog and assigned to the component, making it instantly ready for DataWeave use.

> *Note: Enterprise catalogs may contain a large number of events. The list of available events includes only the subset for which the developer's token has been granted access by the Event Portal administrator. The event name is also prefixed by the Application Domain to provide even more context while browsing the list of events.

To better understand the workflow, consider the following configuration for a "Consume" operator component:

![alt text](/doc/images/EventPortalIntegration-Use.png "Event Portal Integration Example")

1. An Event is selected from the list of available events, `B-mule-PatientContactInfoRequest`. (`1a2mpvr7drzv` is the unique ID of the event)
2. The Event Link is populated, which is the web URL to the Event Portal console. It can be used to directly access all the event details.
3. A queue name is provided, the event can be consumed from this queue.
4. If it doesn't exist yet on the test broker, this queue can be auto-provisioned, saving time for the developer. The queue can be configured to attract and persist messages that have been sent to the topic associated to the event ([Topic-to-queue mapping](https://docs.solace.com/PubSub-Basics/Endpoints.htm#add-topics-to-queues)), which is a recommended pattern for event consumption.
5. The payload's schema is also fetched from the Catalog and is associated with the message MetaData, so it can be used immediately by DataWave. For inbound messaging (like Consume) the "Output", for outbound messaging (like Publish) the "Input" MetaData is populated.

Note that it is optional to use the Event Portal integration—the connector will work regardless. You can also manually overwrite any of the populated fields.

</br>

## Common Parameters

These parameters are similar for all applicable sources and operations, so they are described here for all cases.

#### Event Portal Integration-Related Parameters

These parameters appear in the "General" parameter group.

##### Event Portal Event

| Parameter field | Description |
|---|---|
|Event Portal event | If you have configured a valid EP Token [has been provided](#setting-up-and-getting-access-to-the-pubsub-event-catalog), the dropdown offers you the available events from the Solace PubSub+ Event Portals's Event Catalog, sorted by application domains, then by event name. Select an event to fetch the event details from the Event Catalog. |
|Event Link | Read-only field, populated after an event has been selected. Provides a web link to the selected event in Event Portal Catalog |

#### Provisioning Parameters

This provides an option for the application developer to provision a queue on the PubSub+ Event Broker that is needed for the current interaction, if it doesn't already exist. It saves a manual admin step, which is acceptable on a non-production system. The suggested name of the queue and the related topic name are auto-populated if Event Portal integration is used.

The access mode of the provisioned queue can be set in the [Connector Default Configuration](#connector-defaults-configuration).

##### Provision Option

| Parameter field | Description |
|---|---|
|Provision Queue | Select this option if you want the queue name provided in the component to be provisioned |
|Add Topic Subscription(s) | The specified topic subscription(s) are [added to the queue](https://docs.solace.com/PubSub-Basics/Endpoints.htm#add-topics-to-queues). Note: this parameter is offered only for "Consume" and "Guaranteed Event Listener" components. |

#### Content Type and Encoding Parameters

These parameters appear in the "Content Type and Encoding" parameter group. The "Request-Reply" operation has separate groups for both the Request and the Reply messages.

##### Content Type and Encoding

| Parameter field | Description |
|---|---|
|Content Type | Overrides the default MIME type for the connector for this component |
|Encoding | Overrides the default encoding for the connector for this component |

Refer to the [default encoding](#encoding) and the [default content type](#content-type) settings for the connector.
</br>

## Common Attributes for Inbound Messages

Attributes of inbound Solace messages are automatically available for further processing from each component that can receive messages.

![alt text](/doc/images/InboundMessageAttributes.png "Inbound Message Attributes")

For details of the available properties, refer to the [getter methods](https://docs.solace.com/API-Developer-Online-Ref-Documentation/java/com/solacesystems/jcsmp/XMLMessage.html) in the JCSMP API documentation.

The following additional parameter in the attributes list is a unique, internally generated reference ID for acknowledgement purposes. For more details about the use of this parameter, refer to the [Ack Operation](#ack-operation) section.

| Parameter field | Description |
|---|---|
|messageReferenceId | Use this ID to explicitly acknowledge a message |

</br>

## Operations

### Publish Operation

This operation publishes a direct or a guaranteed message to the PubSub+ Event Broker.

#### Required Parameters

The following minimum parameters are required. All optional parameters use their defaults, which can be seen on the Configuration dialog.

##### Connector Configuration

Selects which connector configuration to use.

##### Destination

The default destination type is a Solace Topic with Direct delivery mode. The name of the destination must be provided.

| Parameter field | Description |
|---|---|
|Delivery Mode | Direct or Persistent. Typically use Direct for Topic type and Persistent for Queue type. Read more in the [Message Delivery Modes](https://docs.solace.com/PubSub-Basics/Core-Concepts-Message-Delivery-Modes.htm) section of the Solace documentation.|
|Type | [Topic](https://docs.solace.com/PubSub-Basics/Understanding-Topics.htm) or [Queue](https://docs.solace.com/PubSub-Basics/Core-Concepts-Endpoints-Queues.htm) |
|Name | The destination's name |
|Provision destination if it doesn't exist (for Queue type only) | If selected, the specified Queue is provisioned on the PubSub+ Event Broker. Note that this is useful for development purposes with test brokers because it doesn't require an extra administrative step to provision the required queues. However, we recommend that production systems be provisioned by administrators. |

#### Optional Parameters

For additional optional parameters, refer to the [Common Parameters](#common-parameters) section.

##### Message

Specifies message content, type and message attributes.

| Parameter field | Description |
|---|---|
|Body | The message contents, can be a Mule expression. Default is `#[payload]`. |
|Message Type | Sets the [JCSMP message type](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Creating-Messages-1.htm?Highlight=Java%20API%20bytesmessage#Types). Select supported types from the dropdown: `BytesMessage`, `TextMessage`, `XMLContentMessage` or Default `Dynamic`, which means that the most appropriate type is automatically determined based on message contents. |
|Additional Message Properties | Select "Edit inline" to provide Solace JCSMP message properties. For details of the available properties, refer to the [setter methods](https://docs.solace.com/API-Developer-Online-Ref-Documentation/java/com/solacesystems/jcsmp/XMLMessage.html) in the JCSMP API documentation. |
|Correlation Id | Sets the correlation ID for [Request-Reply messaging](https://docs.solace.com/PubSub-Basics/Core-Concepts-Message-Models.htm#Request-). The correlation ID is used for correlating a request to a reply. |
|Reply-To Destination | Sets the replyTo destination for the message in Request-Reply messaging. Select "Edit inline" to provide a Reply-To destination name and type. |
|Is Reply Message | Sets the message's reply field, indicating that this message is a reply in Request-Reply messaging.|

#### Example

![alt text](/doc/images/Publish-Example.png "Publish Example")

```xml
<flow name="jsonSendFlow">
  <scheduler doc:name="Scheduler">
    <scheduling-strategy >
      <fixed-frequency frequency="5" timeUnit="SECONDS" startDelay="1"/>
    </scheduling-strategy>
  </scheduler>
  <set-payload value='{"greeting": "hello","name": "solace"}' doc:name="Set Payload" mimeType="application/json"/>
  <solace:publish doc:name="Publish direct" config-ref="Solace_PubSub__Connector_Config" address="t/json/greeting"/>
</flow>
```

For more details, refer to the [JSON Message](../demo/README.md#jsonmessage---complex-message-json) example.

### Consume Operation

This operation returns the next available message from a given Guaranteed Endpoint (Solace queue) from the PubSub+ Event Broker or if there is no message available then wait until timeout is reached and return. If there was an available message there is an option to browse only without consuming (removing) the message from the queue.

#### Required Parameters

The following minimum parameters are required. All optional parameters use their defaults, which can be seen on the Configuration dialog.

##### Connector Configuration

Selects which connector configuration to use.

##### Endpoint

Specifies where to consume the message from and how.

| Parameter field | Description |
|---|---|
|Queue Name | The name of the Solace queue to read the message from |
|Selector | Optionally specify a [selector](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Using-Selectors.htm) to only consume from a subset of messages of interest. |
|Ack Mode | Sets the [Application Acknowledgement](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Acknowledging-Messages.htm) for the received message. Select from `AUTOMATIC IMMEDIATE` (default) or `MANUAL CLIENT`. This setting has no effect in `Browse Only` mode. |
|Browse Only | If set to `True` the available message is not consumed from the queue and is delivered at the next "Consume" operation. In either case, the full message contents are returned for processing. |

>Note: If Ack Mode is "Manual Client", the message must be acknowledged within a [configurable maximum wait time timeout](#general-configuration-advanced-tab), with default value of 2 minutes.

#### Optional Parameters

For additional optional parameters, refer to the [Common Parameters](#common-parameters) section.

##### Time Out

Specifies the maximum wait time for an available message. The default is 1 second.

| Parameter field | Description |
|---|---|
|Time Out | Set the time amount—the default is 1000. |
|Time Unit | Set the time unit—default is `MILLISECONDS`. |

#### Example

![alt text](/doc/images/Consume-Example.png "Consume Example")

```xml
<flow name="consumeFlow">
  <scheduler doc:name="Scheduler">
    <scheduling-strategy >
      <fixed-frequency frequency="5" startDelay="3" timeUnit="SECONDS"/>
    </scheduling-strategy>
  </scheduler>
  <solace:consume doc:name="Consume" config-ref="Solace_PubSub__Connector_Config" address="q/consumeexample"/>
  <logger level="INFO" doc:name="Logger" category="CONSUME OPERATION - " message="#[payload]"/>
</flow>
```

For more details, refer to the [Consume Operation](../demo/README.md#consume---using-consume-operation) example.

### Ack Operation

This operation explicitly acknowledges a guaranteed message using ["Application" Acknowledgement](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Acknowledging-Messages.htm). This acknowledgement indicates that the message has been processed and can be removed from the event broker's queue. This setting is only applicable if `ackMode="MANUAL_CLIENT"` has been set in a previous "Consume" operation or "Guaranteed Endpoint Listener" source.

If no acknowledgement is received within the allowed time, the message times out and becomes available for re-delivery (which is equivalent to a negative acknowledgement). The default timeout is 2 minutes, which can be adjusted in the [General Connection Configuration, Advanced Tab](#general-configuration-advanced-tab).

#### Required Parameters

The following minimum parameters are required. There are no optional parameters.

##### Connector Configuration

Selects which connector configuration to use.

##### Message Reference Id

Specifies the "Reference Id" property from the Solace Message Properties of the message to be acknowledged.

>Note that this is NOT the "Message Id" property!

#### Example

![alt text](/doc/images/Ack-Example.png "Ack Example")

```xml
<flow name="manualackFlow">
  <solace:queue-listener doc:name="Guaranteed Endpoint Listener" config-ref="Solace_PubSub__Connector_Config" address="q/manualack" provisionQueueWithOptionalTopicSubscription="true" ackMode="MANUAL_CLIENT"/>
  <logger level="INFO" doc:name="Logger" message="Got a message" category="MANUALACK - "/>
  <logger level="INFO" doc:name="Processing Delay" message='#[%dw 2.0&#10;import * from dw::Runtime&#10;---&#10;"Acking message after some delay (4s)" wait 4000]' category="MANUALACK - "/>
  <solace:ack doc:name="Ack" config-ref="Solace_PubSub__Connector_Config"/>
  <logger level="INFO" doc:name="Logger" message="#[payload]" category="MANUALACK - "/>
</flow>
```

For more details, refer to the [Manual Acknowledgement](../demo/README.md#manualack---manual-acknowledgement) example.

### Request-Reply Operation

This operation sends a request message as part of a [Request-Reply messaging pattern](https://docs.solace.com/PubSub-Basics/Core-Concepts-Message-Models.htm#Request-) to the PubSub+ Event Broker using either direct or guaranteed messaging. It blocks until a reply is received or a configurable timeout happened.

Depending on the message type, the connector populates the request message with a temporary "Reply-to" Topic or Queue address, as well as an internal "Correlation Id". The connector automatically sets up a listener to the "Reply-to" address and expects the responder to send the reply to this address with the correct Correlation Id.

When using Event Portal integration, the selected event model applies to the **request** message.

#### Required Parameters

The following minimum parameters are required. All optional parameters use their defaults, which can be seen on the Configuration dialog.

##### Connector Configuration

Selects which connector configuration to use.

##### Request Destination

The default destination type is a Solace Topic with Direct delivery mode. The name of the destination must be provided.

| Parameter field | Description |
|---|---|
|Delivery Mode | Direct or Persistent. Typically use Direct for Topic type and Persistent for Queue type. Read more in the [Solace documentation](https://docs.solace.com/PubSub-Basics/Core-Concepts-Message-Delivery-Modes.htm) |
|Type | [Topic](https://docs.solace.com/PubSub-Basics/Understanding-Topics.htm) or [Queue](https://docs.solace.com/PubSub-Basics/Core-Concepts-Endpoints-Queues.htm) |
|Name | The destination's name |

#### Optional Parameters

For additional optional parameters, refer to the [Common Parameters](#common-parameters) section.

>Note: the "Content Type and Encoding" parameters can be separately configured for each the request and the reply message.

##### Request Message

Specifies the request message content, type and message attributes.

| Parameter field | Description |
|---|---|
|Body | The message contents, can be a Mule expression. Default is `#[payload]`. |
|Message Type | Sets the [JCSMP message type](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Creating-Messages-1.htm?Highlight=Java%20API%20bytesmessage#Types). Select supported types from the dropdown: `BytesMessage`, `TextMessage`, `XMLContentMessage` or Default `Dynamic`, which means that the most appropriate type is automatically determined based on message contents. |
|Additional Message Properties | Select "Edit inline" to provide Solace JCSMP message properties. For details of the available properties, refer to the [setter methods](https://docs.solace.com/API-Developer-Online-Ref-Documentation/java/com/solacesystems/jcsmp/XMLMessage.html) in the JCSMP API documentation. |

##### Time Out

Specifies the maximum wait time for the response message. The default is 1 second.

After timeout an error condition is raised which can be handled by an Error Handler.

| Parameter field | Description |
|---|---|
|Time Out | Set the time amount—the default is 1000. |
|Time Unit | Set the time unit—default is `MILLISECONDS`. |

#### Example

![alt text](/doc/images/RequestReply.png "Request-Reply Requestor Example")

```xml
<flow name="requestFlow">
  <scheduler doc:name="Scheduler">
    <scheduling-strategy >
      <fixed-frequency frequency="5" timeUnit="SECONDS" startDelay="3"/>
    </scheduling-strategy>
  </scheduler>
  <solace:request-reply doc:name="Request reply direct" config-ref="Solace_PubSub__Connector_Config" address="t/example/request" timeOut="15" timeUnit="SECONDS">
    <solace:request-message >
      <solace:body ><![CDATA[Hello responder]]></solace:body>
    </solace:request-message>
  </solace:request-reply>
  <logger level="INFO" doc:name="Logger" category="REQUESTOR - " message="#[payload]"/>
  <error-handler>
    <on-error-continue enableNotifications="false" logException="false" doc:name="On Error Continue" type="SOLACE:TIMEOUT">
      <logger level="INFO" doc:name="Logger" message="Oh no"/>
    </on-error-continue>
  </error-handler>
</flow>
```

For more details refer to the [Request Reply](../demo/README.md#requestreply---request-reply) example.

### Recover Session Operation

Allows the user to perform a session recover while consuming an unacknowledged message. For e.g. it can be used with either "Consume Operation" or "Guaranteed Endpoint Listener" with AckMode#MANUAL.

Performing a session recover automatically redelivers all the consumed messages that had not being acknowledged before this recover.

#### Required Parameters

The following minimum parameters are required. There are no optional parameters.

##### Connector Configuration

Selects which connector configuration to use.

##### Message Reference Id

Specifies the "Reference Id" property from the Solace Message Properties of the message to be acknowledged.

>Note that this is NOT the "Message Id" property!

#### Example

![alt text](/doc/images/ListenerRecoverSession.png "Recover Session Example")

```xml
<flow name="listenerrecoversession" doc:id="23279792-da27-47bc-89c4-dd11d057553a" maxConcurrency="1">
  <solace:queue-listener address="q/recoversession" doc:name="Guaranteed Endpoint Listener" doc:id="8431dce7-162c-412c-81e5-ae877e71144a" config-ref="Solace_PubSub__Connector_Config"/>
  <logger level="INFO" doc:name="Logger" doc:id="3db06a3e-959d-4534-9d18-fe5138da4b16" message="Running Flow"/>
  <set-variable value="#[attributes.messageReferenceId]" doc:name="Set Variable" doc:id="01b6baf1-bdce-4a9e-87ec-18ec2741a4ae" variableName="ack_id"/>
  <raise-error doc:name="Raise error" doc:id="7636b0c3-e14a-4d5b-9b3b-73b057f951f8" type="MULE:SECURITY"/>
  <solace:ack doc:name="Ack" doc:id="ee7adb28-feb7-4930-ac9a-d228ea47688d" config-ref="Solace_PubSub__Connector_Config" messageRefId="#[vars.ack_id]"/>
  <error-handler >
    <on-error-propagate enableNotifications="true" logException="true" doc:name="On Error Propagate" doc:id="2825da0f-307f-4a8f-a395-4c924c5b8209">
      <solace:recover-session doc:name="Recover session" doc:id="d141546d-5e97-471e-bbcf-fc1185d33a2f" config-ref="Solace_PubSub__Connector_Config" messageRefId="#[vars.ack_id]"/>
    </on-error-propagate>
  </error-handler>
</flow>
```

For more details refer to the [Recover Session](../demo/README.md#recoversession---recover-session) example.

## Sources

Sources trigger flows every time a message is received.

If performance tuning is required, adjust the [`maxConcurrency`](https://docs.mulesoft.com/mule-runtime/latest/tuning-backpressure-maxconcurrency) parameter of the flow.

### Direct Topic Subscriber Source

This source can be used to asynchronously receive direct messages from the PubSub+ Event Broker that match configured Topic subscription(s).

#### Required Parameters

The following minimum parameters are required. All optional parameters use their defaults, which can be seen on the Configuration dialog.

##### Connector Configuration

Selects which connector configuration to use.

##### Subscriptions

The default destination type is a Solace Topic with Direct delivery mode. The name of the destination must be provided.

| Parameter field | Description |
|---|---|
|Topic(s) | Comma separated list of topic patterns to listen to. For valid wildcard patterns, refer to the list of subscribe topics in the [SMF Topics](https://docs.solace.com/PubSub-Basics/SMF-Topics.htm) section of the Solace documentation. |

#### Optional Parameters

For optional parameters, refer to the [Common Parameters](#common-parameters) section.

#### Example

![alt text](/doc/images/DirectTopicSubscriber.png "Direct Topic Subscriber")

```xml
<flow name="directListener">
  <solace:topic-listener doc:name="Direct Topic Subscriber" config-ref="Solace_PubSub__Connector_Config" topics="t/simple/direct"/>
  <logger level="INFO" doc:name="Logger" category="DIRECT-SOURCE - " message="#[payload]"/>
</flow>
```

For more details, refer to the [Simple Sender Listener](../demo/README.md#simplesenderlistener---listen-for-and-send-messages) example.

### Guaranteed Endpoint Listener Source

This source can be used to asynchronously receive guaranteed messages from the PubSub+ Event Broker from a queue.

#### Required Parameters

The following minimum parameters are required. All optional parameters use their defaults, which can be seen on the Configuration dialog.

##### Connector Configuration

Selects which connector configuration to use.

##### Endpoint

Specifies where to consume the message from and how.

| Parameter field | Description |
|---|---|
|Queue Name | The name of the Solace queue to read the message from |
|Selector | Optionally specify a [selector](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Using-Selectors.htm) to only consume from a subset of messages of interest. |
|Ack Mode | Sets the [Application Acknowledgement](https://docs.solace.com/Solace-PubSub-Messaging-APIs/API-Developer-Guide/Acknowledging-Messages.htm) for the received message. Select from `AUTOMATIC IMMEDIATE` (default), `AUTOMATIC ON FLOW COMPLETION` or `MANUAL CLIENT`. "Manual Client" requires an explicit [Ack operation](#ack-operation) later in the flow. At "Automatic on flow completion" the message is automatically acknowledged after all processing at the end of the flow. |

>Note: If Ack Mode is "Manual Client", the message must be acknowledged within a [configurable maximum wait time timeout](#general-configuration-advanced-tab), with default value of 2 minutes.

#### Optional Parameters

##### Message Processing Parameter [Under "Advanced" tab]
| Parameter field                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Process next message after Flow completion | If enabled then the next received message even if available, will not be delivered to the flow before the complete processing of the previous message has been completed. This is advantageous when you want to prevent race condition to process any additional messages, while using Recover Session. <br/> <em> **Note**: Since this restricts processing of one message at a time for the flow. Any concurrency settings if done, will be of no significance, as essentially the max concurrency will only be one. </em> |

For other optional parameters, refer to the [Common Parameters](#common-parameters) section.

#### Example

![alt text](/doc/images/GuaranteedEndpointListener.png "Guaranteed Endpoint Listener")

```xml
<flow name="guaranteedListener">
  <solace:queue-listener doc:name="Guaranteed Endpoint Listener" config-ref="Solace_PubSub__Connector_Config" address="q/simple"/>
  <logger level="INFO" category="GUARANTEED-SOURCE - " message="#[payload]"/>
</flow>
```

For more details, refer to the [Simple Sender Listener](../demo/README.md#simplesenderlistener---listen-for-and-send-messages) example.

