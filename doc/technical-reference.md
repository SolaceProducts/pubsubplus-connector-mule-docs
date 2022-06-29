<div id="header">

# Solace PubSub+ Connector Module Technical Reference

<div id="toc" class="toc2">

<div id="toctitle">Solace PubSub+ Connector Module</div>

*   [Configurations](#_configurations)
    *   [Config](#config)
*   [Operations](#_operations)
    *   [Ack](#Ack)
    *   [Consume](#Consume)
    *   [Publish](#Publish)
    *   [Request Reply](#requestReply)
*   [Sources](#_sources)
    *   [Guaranteed Endpoint Listener](#queue-listener)
    *   [Direct Topic Subscriber](#topic-listener)
*   [Types](#_types)
    *   [Ack Cache Time Out Parameter](#AckCacheTimeOutParameter)
    *   [Tls](#Tls)
    *   [Trust Store](#TrustStore)
    *   [Key Store](#KeyStore)
    *   [Standard Revocation Check](#standard-revocation-check)
    *   [Custom Ocsp Responder](#custom-ocsp-responder)
    *   [Crl File](#crl-file)
    *   [Reconnection](#Reconnection)
    *   [Reconnect](#reconnect)
    *   [Reconnect Forever](#reconnect-forever)
    *   [Event Portal Config Parameter](#EventPortalConfigParameter)
    *   [Expiration Policy](#ExpirationPolicy)
    *   [Solace Message Properties](#SolaceMessageProperties)
    *   [Redelivery Policy](#RedeliveryPolicy)
    *   [Outbound Additional Message Properties](#OutboundAdditionalMessageProperties)
    *   [Reply To Destination Parameter](#ReplyToDestinationParameter)

</div>

</div>

<div id="content">

<div id="preamble">

<div class="sectionbody">

<div class="paragraph">

A connector for Solace PubSub+ event brokers with native API integration using the JCSMP Java SDK

</div>

</div>

</div>

<div class="sect1">

## Configurations

<div class="sectionbody">

* * *

<div class="sect2">

### Config

<div class="paragraph">

Default configuration

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Name

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name for this configuration. Connectors reference the configuration with this name.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Connection

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Connection](#config_connection)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The connection types that can be provided to this configuration.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Portal Config

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Event Portal Config Parameter](#EventPortalConfigParameter)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

If not configured here (recommended) then the value of the following environment variables are used: 'SOLACE_EVENTPORTAL_API_TOKEN' for Event Portal API Token, 'SOLACE_EVENTPORTAL_CONSOLE_ORG_PREFIX' for Event Portal Console org prefix; this parameter and the env variable shall be left blank/undefined if using default value. Please refer to the documentation if setting non-default Event Portal Console org prefix.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Queue Access Type for provisioned queues

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   EXCLUSIVE

*   NON_EXCLUSIVE

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Access type for automatically provisioned queues

</td>

<td class="tableblock halign-left valign-middle">

EXCLUSIVE

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default encoding of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default contentType of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle">

*/*

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Expiration Policy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Expiration Policy](#ExpirationPolicy)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Configures the minimum amount of time that a dynamic configuration instance can remain idle before the runtime considers it eligible for expiration. This does not mean that the platform will expire the instance at the exact moment that it becomes eligible. The runtime will actually purge the instances when it sees it fit.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### Connection Types

<div class="sect4">

##### Connection

<div class="sect5">

###### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Client Username

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Username to connect to the PubSub+ event broker

</td>

<td class="tableblock halign-left valign-middle">

default

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Client Password

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Password to connect to the PubSub+ event broker

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Message VPN

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The PubSub+ event broker Message VPN that this client should connect to

</td>

<td class="tableblock halign-left valign-middle">

default

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Broker Host

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Address clients use to connect to the PubSub+ event broker to send and receive messages

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Solace Java API session properties

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Object

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Provide key-value pairs for JCSMPProperties or JCSMPChannelProperties (for channel prefix key with CLIENT_CHANNEL_PROPERTIES)

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Maximum wait time for MANUAL_CLIENT Ack

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Ack Cache Time Out Parameter](#AckCacheTimeOutParameter)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Maximum time a non-auto acknowledged message can wait to be acknowledged

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

TLS Configuration

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Tls](#Tls)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

TLS/SSL Configuration to be able to create Secure and Encrypted PubSub+ broker connections

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Reconnection](#Reconnection)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

When the application is deployed, a connectivity test is performed on all connectors. If set to true, deployment will fail if the test doesn't pass after exhausting the associated reconnection strategy

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

<div class="sect3">

#### Associated Operations

<div class="ulist">

*   [Ack](#Ack)

*   [Consume](#Consume)

*   [Publish](#Publish)

*   [Request Reply](#requestReply)

</div>

</div>

<div class="sect3">

#### Associated Sources

<div class="ulist">

*   [Guaranteed Endpoint Listener](#queue-listener)

*   [Direct Topic Subscriber](#topic-listener)

</div>

</div>

</div>

</div>

</div>

<div class="sect1">

## Operations

<div class="sectionbody">

<div class="sect2">

### Ack

<div class="paragraph">

`<solace:ack>`

</div>

<div class="paragraph">

Operation that allows the user to acknowledge successful processing of a message so it can be removed from the PubSub+ event broker.

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Message Reference Id

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The "Reference Id" property from the Solace Message Properties of the message to be acknowledged

</td>

<td class="tableblock halign-left valign-middle">

#[attributes.messageReferenceId]

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

<div class="sect3">

#### Throws

<div class="ulist">

*   SOLACE:GENERIC_ERROR

*   SOLACE:RETRY_EXHAUSTED

*   SOLACE:INVALID_CONFIGURATION

*   SOLACE:CONNECTIVITY

</div>

</div>

</div>

<div class="sect2">

### Consume

<div class="paragraph">

`<solace:consume>`

</div>

<div class="paragraph">

Receive the next available message from a given Guaranteed Endpoint on the PubSub+ event broker. If no message is available, this operation blocks until timeout is reached.

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Portal event

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

ID of the selected event from the Solace PubSub+ Event Portals's Event Catalog, can be empty. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Link

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Web URL link to the selected event. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Queue Name

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The name of the queue to consume from.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Selector

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Optionally specify a selector to only consume from a subset of messages of interest.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Ack Mode

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   AUTOMATIC_IMMEDIATE

*   MANUAL_CLIENT

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

AUTOMATIC_IMMEDIATE: Acknowledgement is sent automatically as message is received. MANUAL_CLIENT: Requires a downstream ACK operation in the flow.

</td>

<td class="tableblock halign-left valign-middle">

AUTOMATIC_IMMEDIATE

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Browse Only

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Read the oldest message on queue without consuming it so that the same message can be delivered to other clients. When true, Ack Mode has no effect as messages are not acknowledged.

</td>

<td class="tableblock halign-left valign-middle">

False

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Provision Queue

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Create queue on first use if it doesn't exist and optionally add topic subscriptions

</td>

<td class="tableblock halign-left valign-middle">

false

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Add Topic Subscription(s)

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

(Optional) Comma separated list of topic subscriptions to add on the queue. Existing topic subscriptions are not affected. No subscription added if empty.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default MIME content type of the message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default encoding of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Time Out

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The timeout value.

</td>

<td class="tableblock halign-left valign-middle">

1000

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Time Unit

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   NANOSECONDS

*   MICROSECONDS

*   MILLISECONDS

*   SECONDS

*   MINUTES

*   HOURS

*   DAYS

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The time unit for the timeout value.

</td>

<td class="tableblock halign-left valign-middle">

MILLISECONDS

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Target Variable

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The name of a variable on which the operation's output will be placed

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Target Value

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

An expression that will be evaluated against the operation's output and the outcome of that expression will be stored in the target variable

</td>

<td class="tableblock halign-left valign-middle">

#[payload]

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### Output

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 50%;"> <col style="width: 50%;"></colgroup>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

**Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Any

</div>

</div>

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

**Attributes Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Solace Message Properties](#SolaceMessageProperties)

</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

<div class="sect3">

#### Throws

<div class="ulist">

*   SOLACE:GENERIC_ERROR

*   SOLACE:INVALID_ENDPOINT

*   SOLACE:RETRY_EXHAUSTED

*   SOLACE:CONNECTIVITY

</div>

</div>

</div>

<div class="sect2">

### Publish

<div class="paragraph">

`<solace:publish>`

</div>

<div class="paragraph">

Operation to publish Direct messages to a Topic or Guaranteed messages to an Endpoint on the PubSub+ event broker.

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Portal event

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

ID of the selected event from the Solace PubSub+ Event Portals's Event Catalog, can be empty. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Link

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

web URL link to the selected event. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Delivery Mode

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   PERSISTENT

*   DIRECT

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The Solace message delivery mode.

</td>

<td class="tableblock halign-left valign-middle">

DIRECT

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   QUEUE

*   TOPIC

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The type of the destination.

</td>

<td class="tableblock halign-left valign-middle">

TOPIC

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Name

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The destination address: the name of the queue or topic.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Provision destination if it doesn’t exist (only applicable to queue type)

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

If enabled the specified queue will be automatically provisioned on the PubSub+ event broker. This has no effect if the destination type is a TOPIC.

</td>

<td class="tableblock halign-left valign-middle">

false

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Message Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   Dynamic

*   BytesMessage

*   TextMessage

*   XMLContentMessage

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The Solace message type that is published. If Dynamic, the most appropriate message type is automatically selected based on the payload dataType.

</td>

<td class="tableblock halign-left valign-middle">

Dynamic

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Additional Message Properties

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Outbound Additional Message Properties](#OutboundAdditionalMessageProperties)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Enables to specify proprietary Solace message properties.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Body

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Any

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The body of the message

</td>

<td class="tableblock halign-left valign-middle">

#[payload]

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Correlation Id

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Sets the correlation ID for Request-Reply messaging. The correlation ID is used for correlating a request to a reply.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reply-To Destination

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Reply To Destination Parameter](#ReplyToDestinationParameter)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Sets the replyTo destination for the message in Request-Reply messaging. Select "Edit inline" to provide a Reply-To destination name and type.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Is Reply Message

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Sets the message's reply field, indicating that this message is a reply in Request-Reply messaging.

</td>

<td class="tableblock halign-left valign-middle">

false

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default MIME content type of the message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default encoding of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

<div class="sect3">

#### Throws

<div class="ulist">

*   SOLACE:GENERIC_ERROR

*   SOLACE:RETRY_EXHAUSTED

*   SOLACE:INVALID_CONFIGURATION

*   SOLACE:CONNECTIVITY

</div>

</div>

</div>

<div class="sect2">

### Request Reply

<div class="paragraph">

`<solace:request-reply>`

</div>

<div class="paragraph">

Operation to send a Request using the Request-Reply messaging pattern. Blocks until a Reply is received or timeout happened.

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Portal event (for Request)

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

ID of the selected event from the Solace PubSub+ Event Portals's Event Catalog, can be empty. Applies to the request message. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Link

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

web URL link to the selected event. Applies to the request message. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Delivery Mode

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   PERSISTENT

*   DIRECT

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The Solace message delivery mode.

</td>

<td class="tableblock halign-left valign-middle">

DIRECT

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   QUEUE

*   TOPIC

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The type of the destination.

</td>

<td class="tableblock halign-left valign-middle">

TOPIC

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Name

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The destination address: the name of the queue or topic.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Message Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   Dynamic

*   BytesMessage

*   TextMessage

*   XMLContentMessage

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The Solace message type that is published. If Dynamic, the most appropriate message type is automatically selected based on the payload dataType.

</td>

<td class="tableblock halign-left valign-middle">

Dynamic

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Additional Message Properties

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Outbound Additional Message Properties](#OutboundAdditionalMessageProperties)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Enables to specify proprietary Solace message properties.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Body

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Any

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The body of the message

</td>

<td class="tableblock halign-left valign-middle">

#[payload]

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default MIME content type of the message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default encoding of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

MIME Content Type, used if the message does not indicate the content type

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Character set the message is encoded in

</td>

<td class="tableblock halign-left valign-middle">

UTF-8

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Time Out

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The timeout value.

</td>

<td class="tableblock halign-left valign-middle">

1000

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Time Unit

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   NANOSECONDS

*   MICROSECONDS

*   MILLISECONDS

*   SECONDS

*   MINUTES

*   HOURS

*   DAYS

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The time unit for the timeout value.

</td>

<td class="tableblock halign-left valign-middle">

MILLISECONDS

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Target Variable

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The name of a variable on which the operation's output will be placed

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Target Value

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

An expression that will be evaluated against the operation's output and the outcome of that expression will be stored in the target variable

</td>

<td class="tableblock halign-left valign-middle">

#[payload]

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### Output

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 50%;"> <col style="width: 50%;"></colgroup>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

**Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Any

</div>

</div>

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

**Attributes Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Solace Message Properties](#SolaceMessageProperties)

</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

<div class="sect3">

#### Throws

<div class="ulist">

*   SOLACE:GENERIC_ERROR

*   SOLACE:TIMEOUT

*   SOLACE:RETRY_EXHAUSTED

*   SOLACE:INVALID_CONFIGURATION

*   SOLACE:CONNECTIVITY

</div>

</div>

</div>

</div>

</div>

<div class="sectionbody">

<div class="sect2">

### Recover Session

<div class="paragraph">

`<solace:recover-session>`

</div>

<div class="paragraph">

Operation that allows  the user to perform a session recover when the AckMode#MANUAL mode is elected while consuming the Message.

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Message Reference Id

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The "Reference Id" property from the Solace Message Properties of the message to be acknowledged

</td>

<td class="tableblock halign-left valign-middle">

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

<div class="sect3">

#### Throws

<div class="ulist">

* SOLACE:GENERIC_ERROR

* SOLACE:SESSION_RECOVER

* SOLACE:RETRY_EXHAUSTED

* SOLACE:INVALID_CONFIGURATION

* SOLACE:CONNECTIVITY

</div>

</div>

</div>

<div class="sect1">

## Sources

<div class="sectionbody">

<div class="sect2">

### Guaranteed Endpoint Listener

<div class="paragraph">

`<solace:queue-listener>`

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Portal event

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The ID of the event used as the model for this message

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Link

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Web URL link to the selected event. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Redelivery Policy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Redelivery Policy](#RedeliveryPolicy)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Defines a policy for processing the redelivery of the same message

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Queue Name

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The name of the queue to consume from.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Selector

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Optionally specify a selector to only consume from a subset of messages of interest.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Ack Mode

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   AUTOMATIC_IMMEDIATE

*   AUTOMATIC_ON_FLOW_COMPLETION

*   MANUAL_CLIENT

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

AUTOMATIC_IMMEDIATE: Acknowledgement is sent automatically as message is received. AUTOMATIC_ON_FLOW_COMPLETION: Acknowledgement is sent automatically on successful completion of the flow. MANUAL_CLIENT: Requires a downstream ACK operation in the flow.

</td>

<td class="tableblock halign-left valign-middle">

AUTOMATIC_IMMEDIATE

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Provision Queue

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Create queue on first use if it doesn't exist and optionally add topic subscriptions

</td>

<td class="tableblock halign-left valign-middle">

false

</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Add Topic Subscription(s)

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

(Optional) Comma separated list of topic subscriptions to add on the queue. Existing topic subscriptions are not affected. No subscription added if empty.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default MIME content type of the message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default encoding of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Process next message after Flow completion 

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Enables delivery of next message to the flow only after previous Flow is completed

</td>

<td class="tableblock halign-left valign-middle">
false
</td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### Output

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 50%;"> <col style="width: 50%;"></colgroup>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

**Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Any

</div>

</div>

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

**Attributes Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Solace Message Properties](#SolaceMessageProperties)

</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

</div>

<div class="sect2">

### Direct Topic Subscriber

<div class="paragraph">

`<solace:topic-listener>`

</div>

<div class="sect3">

#### Parameters

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 20%;"> <col style="width: 35%;"> <col style="width: 20%;"> <col style="width: 5%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Name</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-center valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Configuration

</td>

<td class="tableblock halign-left valign-middle">

String

</td>

<td class="tableblock halign-left valign-middle">

The name of the configuration to use.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Portal event

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The ID of the event used as the model for this message

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Event Link

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Web URL link to the selected event. For display only

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Redelivery Policy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Redelivery Policy](#RedeliveryPolicy)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Defines a policy for processing the redelivery of the same message

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Topic(s)

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

List of topics, comma separated

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle">

**x**

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Content Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default MIME content type of the message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Encoding

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The default encoding of the Message body to be used if the message doesn't communicate it

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A retry strategy in case of connectivity errors

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-center valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### Output

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 50%;"> <col style="width: 50%;"></colgroup>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

**Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Any

</div>

</div>

</td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

**Attributes Type**

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Solace Message Properties](#SolaceMessageProperties)

</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

<div class="sect3">

#### For Configurations.

<div class="ulist">

*   [Config](#config)

</div>

</div>

</div>

</div>

</div>

<div class="sect1">

## Types

<div class="sectionbody">

<div class="sect2">

### Ack Cache Time Out Parameter

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Time Out

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The timeout value

</td>

<td class="tableblock halign-left valign-middle">

2

</td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Time Unit

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   NANOSECONDS

*   MICROSECONDS

*   MILLISECONDS

*   SECONDS

*   MINUTES

*   HOURS

*   DAYS

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The time unit for the timeout value

</td>

<td class="tableblock halign-left valign-middle">

MINUTES

</td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Tls

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Enabled Protocols

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A comma separated list of protocols enabled for this context.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Enabled Cipher Suites

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A comma separated list of cipher suites enabled for this context.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Trust Store

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Trust Store](#TrustStore)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Key Store

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[Key Store](#KeyStore)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Revocation Check

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Standard Revocation Check](#standard-revocation-check)

*   [Custom Ocsp Responder](#custom-ocsp-responder)

*   [Crl File](#crl-file)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Trust Store

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Path

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The location (which will be resolved relative to the current classpath and file system, if possible) of the trust store.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Password

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The password used to protect the trust store.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The type of store used.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Algorithm

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The algorithm used by the trust store.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Insecure

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

If true, no certificate validations will be performed, rendering connections vulnerable to attacks. Use at your own risk.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Key Store

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Path

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The location (which will be resolved relative to the current classpath and file system, if possible) of the key store.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The type of store used.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Alias

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

When the key store contains many private keys, this attribute indicates the alias of the key that should be used. If not defined, the first key in the file will be used by default.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Key Password

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The password used to protect the private key.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Password

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The password used to protect the key store.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Algorithm

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The algorithm used by the key store.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Standard Revocation Check

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Only End Entities

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Only verify the last element of the certificate chain.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Prefer Crls

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Try CRL instead of OCSP first.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

No Fallback

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Do not use the secondary checking method (the one not selected before).

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Soft Fail

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Avoid verification failure when the revocation server can not be reached or is busy.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Custom Ocsp Responder

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Url

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The URL of the OCSP responder.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Cert Alias

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Alias of the signing certificate for the OCSP response (must be in the trust store), if present.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Crl File

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Path

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The path to the CRL file.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Reconnection

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Fails Deployment

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

When the application is deployed, a connectivity test is performed on all connectors. If set to true, deployment will fail if the test doesn’t pass after exhausting the associated reconnection strategy

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reconnection Strategy

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="ulist">

*   [Reconnect](#reconnect)

*   [Reconnect Forever](#reconnect-forever)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The reconnection strategy to use

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Reconnect

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Frequency

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

How often (in ms) to reconnect

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Count

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

How many reconnection attempts to make

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Reconnect Forever

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Frequency

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

How often (in ms) to reconnect

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Event Portal Config Parameter

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Cloud Api Token

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Token to be used to access the PubSub+ Cloud REST API.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Cloud Org Prefix

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Organization prefix for the PubSub+ Cloud Console URL.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Expiration Policy

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Max Idle Time

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A scalar time value for the maximum amount of time a dynamic configuration instance should be allowed to be idle before it’s considered eligible for expiration

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Time Unit

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   NANOSECONDS

*   MICROSECONDS

*   MILLISECONDS

*   SECONDS

*   MINUTES

*   HOURS

*   DAYS

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A time unit that qualifies the maxIdleTime attribute

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Solace Message Properties

| Field	| Type | Description |
|---|---|---|
| DMQ Eligible | Boolean |  |
| Application Message Id | String | An application-specific message identifier |
| Application Message Type | String | An application-specific message type |
| Content Type | String | The HTTP content type header value if the Solace message originated from interaction with an HTTP client or null if it is not set |
| Correlation Id | String | Correlation ID for Request-Reply messaging, used for correlating a request to a reply |
| Cos | Number | The Class of Service (CoS) value for this message |
| Delivery Mode | String | Represents the message delivery mode. Each type is associated with a set of delivery characteristics and guarantees. The valid modes are: NON_PERSISTENT, PERSISTENT and DIRECT  |
| Destination | String | The destination this message was published to |
| Discard Indication | Boolean | True if one or more messages have been discarded prior to the current message, else False |
| Eliding Eligible | Boolean | Whether the message is eligible for eliding |
| Endpoint Type | String | The type of the destination: TOPIC or QUEUE |
| Expiration | Number | The UTC time (in milliseconds, from midnight, January 1, 1970 UTC) when the message is supposed to expire |
| Message Id | String | The Solace message ID |
| Message Reference Id | String | A generated Message Reference ID, to be used for Acknowledgement |
| Priority | Number | The priority value in the range of 0–255, or -1 if it is not set |
| Receiver Timestamp | Number | The receive timestamp (in milliseconds, from midnight, January 1, 1970 UTC) |
| Redelivered | Boolean | Indicates if the message has been delivered by the PubSub+ broker to the API before |
| Reply | Boolean | Whether the message's reply field is set, indicating that this message is a reply |
| Reply To | String | The reply-to destination |
| Reply To Endpoint Type | String | The type of the reply-to endpoint: TOPIC or QUEUE |
| Sender ID | String | The Sender's ID |
| Sender Timestamp | Number | The send timestamp (in milliseconds, from midnight, January 1, 1970 UTC) |
| Sequence Number | Number | The Solace message sequence number |
| Time To Live | Number | The number of milliseconds before the message is discarded or moved to Dead Message Queue |
| User Data | Binary | When an application sends a message, it can optionally attach application-specific data along with the message, such as user data |
| User Properties | Object | The user properties map |

</br>

### Redelivery Policy

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Max Redelivery Count

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The maximum number of times a message can be redelivered and processed unsuccessfully before triggering process-failed-message

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Use Secure Hash

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Whether to use a secure hash algorithm to identify a redelivered message

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Message Digest Algorithm

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The secure hashing algorithm to use. If not set, the default is SHA-256.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Id Expression

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Defines one or more expressions to use to determine when a message has been redelivered. This property may only be set if useSecureHash is false.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Object Store

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

[[ObjectStore]](#ObjectStore)

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The object store where the redelivery counter for each message is going to be stored.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Outbound Additional Message Properties

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Application Message Id

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A string for an application-specific message identifier.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Application Message Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

An application-specific message type

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Cos Value

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The Class of Service (CoS) value for this message.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Dmq Eligible

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Sets the message to be eligible to be moved to a Dead Message Queue.

</td>

<td class="tableblock halign-left valign-middle">

true

</td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Ttl

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The number of milliseconds before the message is discarded or moved to Dead Message Queue.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Expiration

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The UTC time (in milliseconds, from midnight, January 1, 1970 UTC) when the message is supposed to expire.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Eliding Eligible

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Boolean

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Sets whether the message is eligible for eliding.

</td>

<td class="tableblock halign-left valign-middle">

false

</td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Priority

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

A message can optionally have priority set. The valid priority value range is 0-255 with 0 as the lowest priority and 255 as the highest.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

User Properties Map

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Object

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The user properties map.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Sender Id

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

The Sender ID for the message.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Sender Timestamp

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Allows the application to set the send timestamp overriding the API’s generated value.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Sequence Number

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Number

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Sets the sequence number. If sequence number generation is enabled, this method’s argument overrides the generated value.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

User Data

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Binary

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

When an application sends a message, it can optionally attach application-specific data along with the message as user data.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

<div class="sect2">

### Reply To Destination Parameter

<table class="tableblock frame-all grid-all spread"><colgroup><col style="width: 20%;"> <col style="width: 25%;"> <col style="width: 30%;"> <col style="width: 15%;"> <col style="width: 10%;"></colgroup>

<thead>

<tr>

<th class="tableblock halign-left valign-middle">Field</th>

<th class="tableblock halign-left valign-middle">Type</th>

<th class="tableblock halign-left valign-middle">Description</th>

<th class="tableblock halign-left valign-middle">Default Value</th>

<th class="tableblock halign-left valign-middle">Required</th>

</tr>

</thead>

<tbody>

<tr>

<td class="tableblock halign-left valign-middle">

Reply To Destination Name

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

String

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Enables to provide the reply-to address if this is a request message in a Request-Reply messaging pattern.

</td>

<td class="tableblock halign-left valign-middle"></td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

<tr>

<td class="tableblock halign-left valign-middle">

Reply To Destination Type

</td>

<td class="tableblock halign-left valign-middle">

<div>

<div class="paragraph">

Enumeration, one of:

</div>

<div class="ulist">

*   QUEUE

*   TOPIC

</div>

</div>

</td>

<td class="tableblock halign-left valign-middle">

Only applicable if destination name has been specified. Note: Queue type destinations must exist (provisioned) on the PubSub+ broker.

</td>

<td class="tableblock halign-left valign-middle">

TOPIC

</td>

<td class="tableblock halign-left valign-middle"></td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>