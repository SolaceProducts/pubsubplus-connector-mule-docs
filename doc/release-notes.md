# Solace PubSub+ Connector for Mule 4 Release Notes

Use the PubSub+ Connector to leverage PubSub+ Event Broker (event streaming) and PubSub+ Event Portal (event management) within MuleSoft Anypoint Platform, to make your MuleSoft integrations more reliable, agile, and event-driven.  

## v1.1.0
**June 15, 2022**
### Compatibility

| Application/Service | Version |
|---|---|
| Mule Runtime | 4.3 and higher |
| Studio Version | 7.9 and higher |
| PubSub+ Event Broker | 9.1 and higher |
| Java | 1.8 and later |

### Fixed Issues

* On failure, recover and redeliver messages blocking the head of line, when Manual Ack Mode is selected. (SOL-69546)
* Auto recover session when maximum wait time for Manual Ack mode elapses. (SOL-71877)

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
	<version>1.0.0</version>
	<classifier>mule-plugin</classifier>
</dependency>
```

