# Solace PubSub+ Connector for Mule 4 Release Notes

Use the PubSub+ Connector to leverage PubSub+ Event Broker (event streaming) and PubSub+ Event Portal (event management) within Mulesoft Anypoint Platform, to make your Mulesoft integrations more reliable, agile, and event-driven.  

## Solace PubSub+ Connector for Mule 4, v1.0.0

Solace PubSub+ Connector is available on Mule 4.x.

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
*	Request-Reply: a blocking operation that sends a message to a topic or queue (configurable) and waits for a response on an automatically created temporary topic or queue.
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

