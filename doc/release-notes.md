# Solace PubSub+ Connector for Mule 4 Release Notes

Use the PubSub+ Connector to leverage PubSub+ Event Broker (event streaming) and PubSub+ Event Portal (event management) within MuleSoft Anypoint Platform, to make your MuleSoft integrations more reliable, agile, and event-driven.  

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
	<version>1.2.0</version>
	<classifier>mule-plugin</classifier>
</dependency>
```

