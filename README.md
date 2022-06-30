# Solace PubSub+ Connector - Mule 4, Documentation and Examples

v1.0.x

## About the Connector

Use the PubSub+ Connector to leverage PubSub+ Event Broker (event streaming) and PubSub+ Event Portal (event management) within MuleSoft Anypoint Platform, to make your MuleSoft integrations more reliable, agile, and event-driven.

## What's included:

* Native access to PubSub+ Platform in the Mule Palette

*  PubSub+ Event Broker (Seven Operations and Sources supported):
    * Consume event/message (triggered consumer)
    * Direct Topic Subscriber (push direct message consumer)
    * Guaranteed Endpoint Listener (push GM consumer)
    * Request-reply (synchronous wait for reply in flow)
    * Publish to topic or queue (direct or persistent)
    * Ack to acknowledge messages anywhere in the Flow
    * Recover Session to perform session recover while consuming an unacknowledged message

 * PubSub+ Event Portal integration

    * Import events and event schemas
    * Imported event schemas can populate Mule message payload definition
    * Event topic is suggested for definition
    * Deep link back to Event Portal on imported events

## About Solace

Solace helps large enterprises become modern and real-time by giving them everything they need to make their business operations and customer interactions event-driven. With PubSub+, the market’s first and only event management platform, the company provides a comprehensive way to create, document, discover and stream events from where they are produced to where they need to be consumed – securely, reliably, quickly, and guaranteed. Behind Solace technology is the world’s leading group of data movement experts, with nearly 20 years of experience helping global enterprises solve some of the most demanding challenges in a variety of industries – from capital markets, retail, and gaming to space, aviation, and automotive. Established enterprises such as SAP, Barclays and the Royal Bank of Canada, multinational automobile manufacturers such as Groupe Renault and Groupe PSA, and industry disruptors such as Jio use Solace’s advanced event broker technologies to modernize legacy applications, deploy modern microservices, and build an event mesh to support their hybrid cloud, multi-cloud and IoT systems. Learn more at [solace.com](https://solace.com/)

For more information about Solace technology in general please visit these resources:

- The [Solace Developer Portal](https://solace.dev)
- Ask the [Solace community](https://solace.community/)


## About MuleSoft Certified Connectors

MuleSoft Certified Connectors are developed by MuleSoft’s partners and the developer community. These connectors have been reviewed and certified by MuleSoft. To receive assistance or support for the Solace PubSub+ Connector, contact the Solace Developer Community at https://solace.community/ or Solace Support (https://solace.com/support/) if you have a support agreement.
MuleSoft disclaims any support obligation for MuleSoft Certified Connectors. By installing this connector, you consent to MuleSoft sharing your contact information with the developer of this connector so that you can receive more information about it directly from the developer. This 3rd party connector does not require an additional fee to use with MuleSoft Enterprise Edition. MuleSoft Certified Connectors are not accessible to MuleSoft Community Edition.

## Additional References

* [Release Notes](https://github.com/SolaceProducts/pubsubplus-connector-mule-docs/blob/main/doc/release-notes.md)
* [User Guide](https://github.com/SolaceProducts/pubsubplus-connector-mule-docs/blob/main/doc/user-guide.md)
* [Technical Reference](https://github.com/SolaceProducts/pubsubplus-connector-mule-docs/blob/main/doc/technical-reference.md)
* [Examples](https://github.com/SolaceProducts/pubsubplus-connector-mule-docs/blob/main/demo)

## Dependency Snippets

```XML
<dependency>
  <groupId>com.solace.connector</groupId>
  <artifactId>solace-mulesoft-connector</artifactId>
  <version>1.1.0</version>
  <classifier>mule-plugin</classifier>
</dependency>
```

## License

The Solace PubSub+ Connector is licensed under the Solace User Agreement, refer to https://solace.com/license-software/.

Solace PubSub+ Connector Documentation and Examples are licensed under the Apache License, Version 2.0. - See the [LICENSE](https://github.com/SolaceProducts/pubsubplus-connector-mule-docs/blob/main/LICENSE) file for details.

## Type

Connector

## Publishing details

Published by: MuleSoft Partner
</br>

Published on: October, 2021
</br>

Level: MuleSoft-Certified
</br>

Tags: `built-by:solace`, `4.x`, `mulesoft-certified`

