# Solace PubSub+ Connector Examples

This directory contains example flows demonstrating features of the Solace PubSub+ Connector that you can import into Anypoint Studio

It contains the following flows:
* [`simplesenderlistener`](#simplesenderlistener---listen-for-and-send-messages): Listen for and send messages  - direct and guaranteed
* [`consume`](#consume---using-consume-operation): Using consume operation
* [`requestreply`](#requestreply---request-reply): Request-reply with a responder service
* [`manualack`](#manualack---manual-acknowledgement): Manual Message Acknowledgement
* [`jsonmessage`](#jsonmessage---complex-message-json): Complex message - JSON payload

## How to import & run the examples


### Import project(s) into Anypoint Studio

Follow the instructions to import an "Anypoint Studio Project from File System":
[Importing and Exporting Projects](https://docs.mulesoft.com/studio/latest/import-export-packages)

### Run a flow

Review the connector configuration(s) and adjust if necessary.
The projects are configured to connect to a locally deployed broker that doesn't require authentication such as deployed following the instructions for [Using docker](https://docs.solace.com/Solace-SW-Broker-Set-Up/Docker-Containers/Set-Up-Docker-Container-Image.htm).

Then simply run the flow within the project and observe the console/log output in Anypoint Studio to verify the example is running. 
To do this open the Flow XML file in the project, right-click on the canvas and choose the "Run project [...]" menu option

## simplesenderlistener - Listen for and send messages

This example uses simple text payloads.
It contains three flows:
* directListener: a topic subscriber listening on topic `t/simple/direct`, this is configured in the inline `subscriptions` element. You can add wild card subscriptions and add multiple subscriptions.
* guaranteedListener: a queue consumer subscribing to queue `q/simple`. The OnGuaranteedMessage node also sets a subscription to add to the queue - `t/simple/direct`. The queue will be auto provisioned as the `Provision Queue` property on the `Advanced` property sheet is set to "true".
* simplesenderFlow: publishes a text message to `t/simple/direct` triggered periodically by the scheduler node.

**How to verify it is working?** 

Messages received by the two listener flows are logged to the output with a prefix indicating the flow.

```
INFO  2020-07-22 18:08:46,861 [[MuleRuntime].cpuLight.23: [simplesenderlistener].directListener.CPU_LITE @b5ceadc] [event: fe9e4cc0-cc3d-11ea-96db-f018982a62eb] DIRECT-SOURCE - : Hello listeners
INFO  2020-07-22 18:08:46,861 [[MuleRuntime].cpuLight.22: [simplesenderlistener].guaranteedListener.CPU_LITE @6b663d69] [event: fe9ee900-cc3d-11ea-96db-f018982a62eb] GUARANTEED-SOURCE - : Hello listeners
```

## consume - Using consume operation

This example uses simple text payloads.

It contains two flows:
* produceFlow - periodically sends a guaranteed message to queue - `q/consumeexample`. The queue will be auto provisioned as the `Provision Queue` property on the `Advanced` property sheet is set to "true".
* consumeFlow - periodically attempts to receive a message using the consume operation. Note that the scheduler has an initial delay to ensure a message is already enqueued when it's running for the first time.

**How to verify it's working?**

Message received by the consumeFlow are logged ot the console/log output:

```
INFO  2020-07-23 12:23:33,206 [[MuleRuntime].io.11: [consume].consumeFlow.BLOCKING @2480d044] [event: f1510d71-ccd6-11ea-a970-f018982a62eb] CONSUME OPERATION - : Hello Consume
```

## requestreply - Request reply

Demonstrates request-reply with direct messaging.

This example uses simple text payloads.

It contains two flows:
* responderFlow: listens for messages on a topic `t/example/request`, logs the message received and sends a response message to a requestor. Note that the `Publish direct` operation assigns the topic and correlation id from the metadata of the received message so the message is routed to the requestor. It also makrs the message as a reply.
* requestFlow: periodically sends a request and attempts to receive a response. The process logs the response received from `requestorFlow`

**How to verify it's working?**

The `requestFlow` logs the reponse it receives, the `responderFlow` logs the request it receives before sending a response.
You will observe logging in the console/log output similar to this: 

```
INFO  2020-07-23 12:30:13,760 [[MuleRuntime].cpuLight.22: [requestreply].responderFlow.CPU_LITE @f48751b] [event: e012e0f0-ccd7-11ea-94f4-f018982a62eb] RESPONDER - : Hello responder
INFO  2020-07-23 12:30:13,764 [[MuleRuntime].io.11: [requestreply].requestFlow.BLOCKING @4ae4b7bc] [event: e0126bc1-ccd7-11ea-94f4-f018982a62eb] REQUESTOR - : This is the response
```


## manualack - Manual acknowledgement

Demonstrates manually acknowledging messages.

This example uses simple text payloads.

Please inspect the connector configuguration's `Endpoint Consumer` tab - the acknowledgement mode is set to `MANUAL_CLIENT`.

It contains two flows:
* manualackFlow: listens for guaranteed messages on queue `q/manualack`, it's set to "Auto provision" queue. The process first logs that it receives a message, then waits for 4 seconds, logs that it acknowledges the message and acks the message, then logs the message payload.
* publishFlow: publishes a message periodically (every 30 seconds) 

**How to verify it's working?**

On every message the `manualackFlow` writes the followng entries into the log - note that the "Got a message" and "acking message" log entries will be around 4 seconds apart as outlined above:

```
INFO  2020-07-23 13:23:03,129 [[MuleRuntime].cpuLight.21: [manualack].manualackFlow.CPU_LITE @47b98f2c] [event: 412a3580-ccdf-11ea-98f0-f018982a62eb] MANUALACK - : Got a message
INFO  2020-07-23 13:23:07,130 [[MuleRuntime].cpuLight.21: [manualack].manualackFlow.CPU_LITE @47b98f2c] [event: 412a3580-ccdf-11ea-98f0-f018982a62eb] MANUALACK - : Acking message
INFO  2020-07-23 13:23:07,132 [[MuleRuntime].io.11: [manualack].manualackFlow.BLOCKING @1ed230b5] [event: 412a3580-ccdf-11ea-98f0-f018982a62eb] MANUALACK - : A guaranteed message
```

You can monitor the queue in the Solace admin UI and should be able to observe a message in the queu if you check straight when "Got a message" was logged.

## jsonmessage - Complex Message, JSON

Demonstrates a process with a JSON message and how this can be easily serialised and de-serialised.

It contains two flows:
* jsonSendFlow: this process runs periodically, sets a JSON payload and then publishes it as a direct message. Note:
    * The `Set Payload` node specifies the json payload - `{"greeting": "hello","name": "solace"}` and sets the MIME type to `application\json`. It's payload output metadat is set to a `HelloWorld` message type which represents the schema of the JSON payload.
    * `Publish direct` uses the payload created by `Set Payload` as its message body. It doesn't specify a MIME type and sends it to `t/json/greeting`. The metadata of the input payload is set to the `HelloWorld` type described above.
* jsonListenerFlow: triggered on receipt of a message on the topic `t/json/greeting`. The metadata of the output payload of `OnDirectMessage` is also set to the `HelloMessage` type. The `Logger` outputs the name attribute of the JSON message. 

**How to verify it's working?**

On every message `jsonListenerFlow` outputs the `name` attribute of the JSON payload received:
```
INFO  2020-07-23 13:35:50,946 [[MuleRuntime].cpuLight.15: [jsonmessage].jsonListenerFlow.CPU_LITE @560e1345] [event: 0ad1a200-cce1-11ea-86ea-f018982a62eb] greeting to: solace
```

## recoversession - Recover Session

Demonstrates recover session when the AckMode#MANUAL mode is selected for a Consume Operation.

This example uses simple text payloads.

Please inspect the connector configuration's `Endpoint Consumer` tab - the acknowledgement mode is set to `MANUAL_CLIENT`.

It contains the following flows:
* ConsumeOperationRecoverSession: Consumes message from the queue `q/recoversession` on schedule. Ensure this queue is provisioned and queued with some messages. The process first logs that the flow is running, then consumes messages from the queue, sets to a variable then finally throws MULE:SECURITY error. This error is then handled using "On Error Propagate" within which a Recover Session operation is called with Message Reference Id.

**How to verify it's working?**

On every message the `ConsumeOperationRecoverSession` writes the followng entries into the log - note that the "Running Flow", followed by "MULE:SECURITY" and "Recover session called for msgRefId=...", "Successfully restarted consume operation" and finally the Flow starts again:

```
INFO  2022-06-28 22:28:08,191 [[MuleRuntime].uber.03: [solacepubsubplusconnector].ConsumeOperationRecoverSession.CPU_LITE @71bbb8f8] [processor: ConsumeOperationRecoverSession/processors/0; event: 7c27fa00-f703-11ec-aa67-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Running Flow
ERROR 2022-06-28 22:28:08,789 [[MuleRuntime].uber.04: [solacepubsubplusconnector].ConsumeOperationRecoverSession.BLOCKING @5703f70e] [processor: ConsumeOperationRecoverSession/processors/3; event: 7c27fa00-f703-11ec-aa67-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : ConsumeOperationRecoverSession/processors/3 @ solacepubsubplusconnector:solacepubsubplusconnector.xml:257 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="7636b0c3-e14a-4d5b-9b3b-73b057f951f8" type="MULE:SECURITY"></raise-error>
Error type            : MULE:SECURITY
FlowStack             : at ConsumeOperationRecoverSession(ConsumeOperationRecoverSession/processors/3 @ solacepubsubplusconnector:solacepubsubplusconnector.xml:257 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-06-28 22:28:08,789 [[MuleRuntime].uber.03: [solacepubsubplusconnector].ConsumeOperationRecoverSession.BLOCKING @5703f70e] [processor: ConsumeOperationRecoverSession/errorHandler/0/processors/0; event: 7c27fa00-f703-11ec-aa67-66bc5810864b] com.solace.connector.mulesoft.internal.operation.RecoverOperation: Recover session called for msgRefId=6d130d4a-80f9-4a39-982b-05c2083e5ba1
DEBUG 2022-06-28 22:28:08,789 [[MuleRuntime].uber.03: [solacepubsubplusconnector].ConsumeOperationRecoverSession.BLOCKING @5703f70e] [processor: ConsumeOperationRecoverSession/errorHandler/0/processors/0; event: 7c27fa00-f703-11ec-aa67-66bc5810864b] com.solace.connector.mulesoft.internal.connection.SolaceConnection: Closing flowReceiver
INFO  2022-06-28 22:28:09,010 [[MuleRuntime].uber.03: [solacepubsubplusconnector].ConsumeOperationRecoverSession.BLOCKING @5703f70e] [processor: ConsumeOperationRecoverSession/errorHandler/0/processors/0; event: 7c27fa00-f703-11ec-aa67-66bc5810864b] com.solace.connector.mulesoft.internal.connection.SolaceConnection: Successfully restarted consume operation
INFO  2022-06-28 22:28:18,181 [[MuleRuntime].uber.04: [solacepubsubplusconnector].ConsumeOperationRecoverSession.CPU_LITE @71bbb8f8] [processor: ConsumeOperationRecoverSession/processors/0; event: 82210f50-f703-11ec-aa67-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Running Flow
```

You can monitor the queue provided it has the default "infinite redeliveries" property set, the messages never get consumed.
