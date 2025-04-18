# Solace PubSub+ Connector for Mule 4 Release Notes

Use the PubSub+ Connector to leverage PubSub+ Event Broker (event streaming) and PubSub+ Event Portal (event management) within MuleSoft Anypoint Platform, to make your MuleSoft integrations more reliable, agile, and event-driven.

## v1.7.0
**Mar 14, 2025**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### New Features
* Enhanced the connector to support HTTP-based content encoding types (like gzip) by allowing direct passthrough of encoding values without attempting a strict charset conversion

### Fixed Issues
* Fixed an incompatibility when accessing the boolean type properties in SolaceMessageProperties within Dataweave expressions while running on Java 17


## v1.6.0
**Nov 30, 2024**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### New Features
1. Negative Acknowledgement (NACK) Support <br/>
   • Introduced for Guaranteed Endpoint Listener and Guaranteed Endpoint Polling Listener Sources<br/>
   • Uses the FAILED settlement outcome<br/>
2. New Nack Operation <br/>
   • Allows negative acknowledgement of guaranteed messages <br/>
   • Supports both FAILED and REJECTED settlement outcomes <br/> 
   • Recommended alternative to Recover Session Operation settlement outcomes <br/>
3. Added support for DeliveryCount and ReplicationGroupMessageId as Solace Message Properties <br/>

**Note:** The version of the Solace PS+ broker that supports NACK using FAILED & REJECTED settlement outcome is version 10.2.1 and later. However, it maintains compatibility with brokers that do not support these settlement capabilities. This capability is automatically processed internally to the connector and requires no user interaction or configuration.

### Deprecation Notice
The Recover Session Operation feature will soon be deprecated. Users are advised to transition to the new Nack Operation for improved message handling and error management.

## v1.5.0
**May 31, 2024**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### New Features
* Added Java 17 support

### Fixed Issues
* Fixed issue when using OAuth connection with Mule Reconnection Strategy which previously would cause a bootloop for disconnections over then 15 seconds.

## v1.4.1
**March 01, 2024**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### Fixed Issues
* Fixed the flow recover session to work in-sync with the Circuit Breaker config

## v1.4.0
**November 15, 2023**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### New Features
* We've introduced support for the OAuth 2.0 Client Credentials grant type under Security Tab. This allows applications to request an access token using their client credentials (Client ID and Client Secret) to authenticate against the authorization server and get a token directly. This strategy is particularly beneficial for long-lived and stable connections. (SOL-104703)
* JCSMP Properties now supports Mule Expression Language (MEL).

### Fixed Issues
* Now Solace connector support non-repeatable streams payload during retries (SOL-103111).
* Removed unnecessary dependency

## v1.3.1
**July 07, 2023**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### Fixed Issues

* JCSMP update from 10.16 to 10.21 to include JCMSP bugfixes.

## v1.3.0
**June 30, 2023**
### Compatibility

| Application/Service  | Version         |
|----------------------|-----------------|
| Mule Runtime         | 4.3 and higher  |
| Studio Version       | 7.9 and higher  |
| PubSub+ Event Broker | 9.1 and higher  |
| Java                 | 1.8 and later   |

### Fixed Issues

* Fixed the issue with Mule Runtime 4.3 (mule-maven-plugin version 3.6.0 and above) and Mule Runtime 4.4. (SOL-96888)

**Note:** <em>It was necessary to change some config elements of the Guaranteed Endpoint Polling Listener to address the issue mentioned as fixed above. Therefore, you must remove any existing Guaranteed Endpoint Polling Listeners from your Mule flows, save the project, and then add new Guaranteed Endpoint Polling Listeners in your flows, as needed. This is only necessary for flows created before upgrading to v1.2.1 of the Solace connector. </em>

## v1.2.0
**February 08, 2023**
### Compatibility

| Application/Service  | Version                                                                                                                                                                           |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Mule Runtime         | 4.3 - with `mule-maven-plugin` version 3.5.4<br/> 4.4 - see [Known Issues](#known-issues). This connector will not work in Mule Runtime 4.4 until a MuleSoft issue has been fixed |
| Studio Version       | 7.9 and higher                                                                                                                                                                    |
| PubSub+ Event Broker | 9.1 and higher                                                                                                                                                                    |
| Java                 | 1.8 and later                                                                                                                                                                     |

### Fixed Issues

* Fixed the unbinding of connector on every message received when Ack Mode is set to AUTOMATIC_ON_FLOW_COMPLETION or AUTOMATIC_IMMEDIATE. (SOL-79397)
* Multiple start/stop of Guaranteed Endpoint Listener sourced flows is now fixed. (SOL-73393)
* Reconnection strategy is now fixed. The default reconnect retries will now be 0 instead of infinite. (SOL-65619)
* Destination Name in Publish and Request Reply Operation is now fixed. Requires the attribute name `destination-type` to be updated to `destinationType` in the Mule Project XML file for Publish and Request Reply Operations. (SOL-84858)
* Upgraded library dependency `com.solacesystems:sol-jcsmp` to 10.16.0.
* Recover Session on error, is now by default without explicit error handling when consuming in AUTOMATIC_ON_FLOW_COMPLETION and MANUAL_CLIENT.

### New Features

**Sources**

* Added a new source type "Guaranteed Endpoint Polling Listener" which retrieves fixed number of messages at the scheduled polling interval and dispatches to the flow individually.
* Added support for Error Handling using the Circuit Breaker pattern for the "Guaranteed Endpoint Polling Listener" and "Guaranteed Endpoint Listener".

**Operations**

* Support payload types of CursorStreamProvider and OutputHandler for Publish Operation. (SOL-61581)
* Added support for message consumption published with Content-Type as "text". (SOL-74548)

### Known Issues

* There is an existing issue when running Mule Runtime 4.4 or 4.3 with `mule-maven-plugin` version post version 3.5.4, resulting in an error message reading `Cannot resolve the name 'mule:global-abstract-scheduling-strategy' to a(n) 'element declaration' component...`. This has been reported to MuleSoft and has been acknowledged as an issue in the `mule-maven-plugin`. Solace is unable to work around this issue in Mule Runtime 4.4 and therefore this version cannot be used until MuleSoft has a fix. For version 4.3 of the Mule Runtime, you can downgrade the `mule-maven-plugin` to version 3.5.4 (or lower) and the Solace Connector v1.2 will work as expected.

## v1.1.0
**August 1, 2022**
### Compatibility

| Application/Service | Version |
|---|---|
| Mule Runtime | 4.3 and higher |
| Studio Version | 7.9 and higher |
| PubSub+ Event Broker | 9.1 and higher |
| Java | 1.8 and later |

### Fixed Issues

* On failure, recover and redeliver messages which are unacknowledged. (SOL-69546)
* Auto recover session when maximum wait time for Manual Ack mode elapses. (SOL-71877)

### Key Features

**Sources**

*	Guaranteed Endpoint Listener: If enabled then the next received message even if available, will not be delivered to the flow before the complete processing of the previous message has been completed. This is advantageous when you want to prevent race condition to process any additional messages, while using Recover Session.

## v1.0.2
**May 4, 2022**
### Compatibility

| Application/Service | Version |
|---|---|
| Mule Runtime | 4.3 and higher |
| Studio Version | 7.9 and higher |
| PubSub+ Event Broker | 9.1 and higher |
| Java | 1.8 and later |

### Fixed Issues

* Connector leaks egress flows when no message is available in the consume operation. (SOL-67905)

## v1.0.1
**December 17, 2021**
### Compatibility

| Application/Service | Version |
|---|---|
| Mule Runtime | 4.3 and higher |
| Studio Version | 7.9 and higher |
| PubSub+ Event Broker | 9.1 and higher |
| Java | 1.8 and later |

### Fixed Issues

* The DMQ is now only provisioned by consuming operations when the ‘Provision Queue’ option is enabled. (SOL-59876)
* Default producer is now lazily initialized by publishing operations only. (SOL-59879)

## v1.0.0
**October 21, 2021**
### Compatibility

| Application/Service | Version |
|---|---|
| Mule Runtime | 4.3 and higher |
| Studio Version | 7.9 and higher |
| PubSub+ Event Broker | 9.1 and higher |
| Java | 1.8 and later |


### Key Features

**Design-time**

*	PubSub+ Event Portal: integration with the Event Catalog in PubSub+ Event Portal
*	Automatic Queue Provisioning: option to automatically provision Solace queues on test PubSub+ event brokers and then add topic subscriptions to those queues (specified at design-time but actually created at runtime)

**Sources**

*	Direct Topic Subscriber: triggered when a direct message is received on a set of topic subscriptions
*	Guaranteed Endpoint Listener: triggered when a guaranteed message is received on a queue

**Operations**

*	Publish: publishes a direct message to a topic or a persistent message to a queue
*	Consume: consumes a single guaranteed message from an endpoint (Solace queue)
*	Request-Reply: a blocking operation that sends a message to a topic or queue (configurable) and waits for a response on an automatically created temporary topic or queue
*	Ack: acknowledges receipt of a guaranteed message

### POM Dependency

Add this dependency to your application pom.xml
```
<dependency>
	<groupId>com.solace.connector</groupId>
	<artifactId>solace-mulesoft-connector</artifactId>
	<version>1.6.0</version>
	<classifier>mule-plugin</classifier>
</dependency>
```

