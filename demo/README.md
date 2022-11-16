# Solace PubSub+ Connector Examples

This directory contains example flows demonstrating features of the Solace PubSub+ Connector that you can import into Anypoint Studio

It contains the following flows:
* [`simplesenderlistener`](#simplesenderlistener---listen-for-and-send-messages): Listen for and send messages  - direct and guaranteed
* [`consume`](#consume---using-consume-operation): Using consume operation
* [`requestreply`](#requestreply---request-reply): Request-reply with a responder service
* [`manualack`](#manualack---manual-acknowledgement): Manual Message Acknowledgement
* [`jsonmessage`](#jsonmessage---complex-message-json): Complex message - JSON payload
* [`listenerrecoversession`](#recoversession---recover-session): Recover Session

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

Demonstrates recover session when consuming an unacknowledged message.

This example uses simple text payloads.

Please inspect the connector configuration's `Endpoint` tab - the acknowledgement mode is recommended to be set to `MANUAL_CLIENT` in this case.

It contains the following flows:
* listenerrecoversession: Listener source consumes message from the queue `q/recoversession`. Ensure this queue is provisioned and queued with a message as "Test message - 1". The process first logs that the flow is running, then consumes messages from the queue, sets to a variable then finally throws MULE:SECURITY error. This error is then handled using "On Error Propagate" within which a Recover Session operation is called with Message Reference Id.

**How to verify it's working?**

On every message the `listenerrecoversession` writes the following entries into the log - note that the "Running flow with message payload: Test message - 1", followed by "MULE:SECURITY" and "Recover session called for msgRefId=...", "Successfully restarted consume operation" and finally the Flow starts again with the same message:

```
INFO  2022-07-07 13:20:41,866 [[MuleRuntime].uber.20: [solacepubsubplusconnector].listenerrecoversession.BLOCKING @5d091111] [processor: listenerrecoversession/errorHandler/0/processors/0; event: 7f91fb10-fdc9-11ec-ab7a-66bc5810864b] com.solace.connector.mulesoft.internal.connection.SolaceConnection: Successfully restarted source consumer
INFO  2022-07-07 13:20:42,254 [[MuleRuntime].uber.20: [solacepubsubplusconnector].listenerrecoversession.CPU_LITE @55b475fe] [processor: listenerrecoversession/processors/0; event: 803036e0-fdc9-11ec-ab7a-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Running flow with message payload: Test message - 1
ERROR 2022-07-07 13:20:42,254 [[MuleRuntime].uber.20: [solacepubsubplusconnector].listenerrecoversession.CPU_LITE @55b475fe] [processor: listenerrecoversession/processors/2; event: 803036e0-fdc9-11ec-ab7a-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : listenerrecoversession/processors/2 @ solacepubsubplusconnector:solacepubsubplusconnector.xml:251 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="7636b0c3-e14a-4d5b-9b3b-73b057f951f8" type="MULE:SECURITY"></raise-error>
Error type            : MULE:SECURITY
FlowStack             : at listenerrecoversession(listenerrecoversession/processors/2 @ solacepubsubplusconnector:solacepubsubplusconnector.xml:251 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-07-07 13:20:42,254 [[MuleRuntime].uber.11: [solacepubsubplusconnector].listenerrecoversession.BLOCKING @5d091111] [processor: listenerrecoversession/errorHandler/0/processors/0; event: 803036e0-fdc9-11ec-ab7a-66bc5810864b] com.solace.connector.mulesoft.internal.operation.RecoverOperation: Recover session called for msgRefId=f898f475-8514-47db-8adb-c45b0f5082e8
INFO  2022-07-07 13:20:42,900 [[MuleRuntime].uber.11: [solacepubsubplusconnector].listenerrecoversession.BLOCKING @5d091111] [processor: listenerrecoversession/errorHandler/0/processors/0; event: 803036e0-fdc9-11ec-ab7a-66bc5810864b] com.solace.connector.mulesoft.internal.connection.SolaceConnection: Successfully restarted source consumer
INFO  2022-07-07 13:20:43,229 [[MuleRuntime].uber.11: [solacepubsubplusconnector].listenerrecoversession.CPU_LITE @55b475fe] [processor: listenerrecoversession/processors/0; event: 80c4fcd0-fdc9-11ec-ab7a-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Running flow with message payload: Test message - 1
```

You can monitor the queue provided it has the default "infinite redeliveries" property set, the message never gets consumed.

## pollinglistenerfixedfrequency - Polling Listener Fixed Frequency

Demonstrates Guaranteed Endpoint Polling Listener with Fixed Frequency (Ack Mode is set to MANUAL_CLIENT).

This example uses simple text payloads.

Test set-up:

Ensure a queue is provisioned with name as "test/q". Publish some messages to this queue. In the test example, 8 messages are published with sequence information in the payload as "Test msg 1" to "Test msg 8".
Since the schedule is Fixed Frequency type with frequency as 30 seconds and initial start delay of 1 second. Expectation would be to see maximum of 5 messages consumed at every schedule.

**How to verify it's working?**

On every message consumed, the `pollinglistenerfixedfrequency` writes the following entries into the log - note the timestamps of log message with message prefix as `Received Message:`. The first 5 messages are logged at 16:19:37 and next 3 are logged after 30 seconds i.e., 16:20:07. This confirms the scheduled consume with a fetch size of 5:

```
INFO  2022-11-16 16:19:37,745 [[MuleRuntime].uber.02: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 601aad20-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 5
INFO  2022-11-16 16:19:37,745 [[MuleRuntime].uber.08: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 6017ee00-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 1
INFO  2022-11-16 16:19:37,745 [[MuleRuntime].uber.07: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 601a5f00-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 2
INFO  2022-11-16 16:19:37,745 [[MuleRuntime].uber.03: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 601a8611-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 4
INFO  2022-11-16 16:19:37,745 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 601a8610-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 3
INFO  2022-11-16 16:20:07,899 [[MuleRuntime].uber.01: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 721863a0-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 6
INFO  2022-11-16 16:20:07,899 [[MuleRuntime].uber.06: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 721863a1-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 7
INFO  2022-11-16 16:20:07,899 [[MuleRuntime].uber.04: [solpubsub1dot2].pollinglistener.CPU_LITE @3e3695d] [processor: pollinglistener/processors/0; event: 72188ab0-65f4-11ed-9fd1-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 8
```

## pollinglistenercronexpression - Polling Listener Cron Expression

Demonstrates Guaranteed Endpoint Polling Listener with Cron Expression (Ack Mode is set to MANUAL_CLIENT).

This example uses simple text payloads.

Test set-up:

Ensure a queue is provisioned with name as "test/q". Publish some messages to this queue. In the test example, 8 messages are published with sequence information in the payload as "Test msg 1" to "Test msg 8".
Since the schedule is Cron Expression, it is scheduled to run every 30 seconds of the day, every day and the Timezone is `America/Los_Angeles`. Expectation would be to see maximum of 5 messages consumed at every schedule.

**How to verify it's working?**

On every message consumed, the `pollinglistenercronexpression` writes the following entries into the log - note the timestamps of log message with message prefix as `Received Message:`. The first 5 messages are logged at 16:26:31 and next 3 are logged after 30 seconds i.e., 16:27:01. This confirms the scheduled consume with a fetch size of 5:

```
INFO  2022-11-16 16:26:31,085 [[MuleRuntime].uber.09: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 56789790-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 3
INFO  2022-11-16 16:26:31,085 [[MuleRuntime].uber.06: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 5675b160-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 1
INFO  2022-11-16 16:26:31,085 [[MuleRuntime].uber.11: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 5678bea1-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 5
INFO  2022-11-16 16:26:31,085 [[MuleRuntime].uber.03: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 5678bea0-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 4
INFO  2022-11-16 16:26:31,085 [[MuleRuntime].uber.07: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 56787080-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 2
INFO  2022-11-16 16:27:01,226 [[MuleRuntime].uber.06: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 68751590-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 6
INFO  2022-11-16 16:27:01,226 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 68751591-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 7
INFO  2022-11-16 16:27:01,226 [[MuleRuntime].uber.10: [solpubsub1dot2].pollinglistenerfixedfrequency.CPU_LITE @4127790] [processor: pollinglistenerfixedfrequency/processors/0; event: 68753ca0-65f5-11ed-9d38-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 8
```

## pollinglistenercircuitbreaker - Polling Listener Fixed Frequency with Circuit Breaker

Demonstrates Guaranteed Endpoint Polling Listener with Fixed Frequency and Circuit Breaker configured (Ack Mode is set to MANUAL_CLIENT).

>Note: MaxConcurrency is set to 1, as we want to perform a deterministic testing.

This example uses simple text payloads.

Test set-up:

Ensure a queue is provisioned with name as "test/q". Publish some messages to this queue. In the test example, more than 3 messages are published with sequence information in the payload starting with "Test msg 1".
The schedule is "Fixed Frequency" type with frequency as 30 seconds and initial start delay of 1 second. The inline circuit breaker is configured with a custom error type `DUMMY_HTTP:CONNECTIVITY` and errorThreshold is set to 3. With a tripTimeout of 60 seconds, we should see that once the circuit is moved to OPEN state, the half-test should be performed only after 60 seconds of tripping.
Also we should observe that the test is done with only 1 message and if it fails the circuit is still open. We might not be able to test the circuit state "Closed" after it is "Open", because of recovery of the failing service has to be dynamic, which is not possible with this test flow.

**How to verify it's working?**

If we observer the below logs, after the third message fails with `DUMMY_HTTP:CONNECTIVITY` the circuit is moved to OPEN state. `Circuit tripping detected at failureCount=3` logged at 16:44:18 and the half-open test is performed at 16:45:48 which is after 60 seconds provided the next polling is scheduled. This half-test continues at the same internal until the test passes:

```
INFO  2022-11-16 16:44:18,452 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/0; event: d29ff820-65f7-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 1
ERROR 2022-11-16 16:44:18,527 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: d29ff820-65f7-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="789faea3-3c3c-495b-b4fb-5a012d9cb7f0" type="DUMMY_HTTP:CONNECTIVITY"></raise-error>
Error type            : DUMMY_HTTP:CONNECTIVITY
FlowStack             : at pollinglistenercircuitbreaker(pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-11-16 16:44:18,574 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/0; event: d2a48c00-65f7-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 2
ERROR 2022-11-16 16:44:18,575 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: d2a48c00-65f7-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="789faea3-3c3c-495b-b4fb-5a012d9cb7f0" type="DUMMY_HTTP:CONNECTIVITY"></raise-error>
Error type            : DUMMY_HTTP:CONNECTIVITY
FlowStack             : at pollinglistenercircuitbreaker(pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-11-16 16:44:18,578 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/0; event: d2be5590-65f7-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 3
ERROR 2022-11-16 16:44:18,579 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: d2be5590-65f7-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="789faea3-3c3c-495b-b4fb-5a012d9cb7f0" type="DUMMY_HTTP:CONNECTIVITY"></raise-error>
Error type            : DUMMY_HTTP:CONNECTIVITY
FlowStack             : at pollinglistenercircuitbreaker(pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-11-16 16:44:18,580 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: d2be5590-65f7-11ed-85fb-66bc5810864b] com.solace.connector.mulesoft.api.circuit.DefaultCircuitBreaker: Circuit tripping detected at failureCount=3
INFO  2022-11-16 16:44:18,580 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: d2be5590-65f7-11ed-85fb-66bc5810864b] com.solace.connector.mulesoft.api.circuit.DefaultCircuitBreaker: Circuit '_Solace_Polling_Consumer_pollinglistenercircuitbreaker' tripped, moving to OPEN
INFO  2022-11-16 16:45:48,320 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/0; event: 0841faf0-65f8-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 1
ERROR 2022-11-16 16:45:48,320 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: 0841faf0-65f8-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="789faea3-3c3c-495b-b4fb-5a012d9cb7f0" type="DUMMY_HTTP:CONNECTIVITY"></raise-error>
Error type            : DUMMY_HTTP:CONNECTIVITY
FlowStack             : at pollinglistenercircuitbreaker(pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-11-16 16:45:48,320 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: 0841faf0-65f8-11ed-85fb-66bc5810864b] com.solace.connector.mulesoft.api.circuit.DefaultCircuitBreaker: Circuit tripping detected at failureCount=4
INFO  2022-11-16 16:45:48,321 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: 0841faf0-65f8-11ed-85fb-66bc5810864b] com.solace.connector.mulesoft.api.circuit.DefaultCircuitBreaker: Circuit '_Solace_Polling_Consumer_pollinglistenercircuitbreaker' tripped, moving to OPEN
INFO  2022-11-16 16:47:18,322 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/0; event: 3de73210-65f8-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.processor.LoggerMessageProcessor: Received Message: Test msg 1
ERROR 2022-11-16 16:47:18,322 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: 3de73210-65f8-11ed-85fb-66bc5810864b] org.mule.runtime.core.internal.exception.OnErrorPropagateHandler: 
********************************************************************************
Message               : An error occurred.
Element               : pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error)
Element DSL           : <raise-error doc:name="Raise error" doc:id="789faea3-3c3c-495b-b4fb-5a012d9cb7f0" type="DUMMY_HTTP:CONNECTIVITY"></raise-error>
Error type            : DUMMY_HTTP:CONNECTIVITY
FlowStack             : at pollinglistenercircuitbreaker(pollinglistenercircuitbreaker/processors/1 @ solpubsub1dot2:pollinglistener.xml:21 (Raise error))

  (set debug level logging or '-Dmule.verbose.exceptions=true' for everything)
********************************************************************************

INFO  2022-11-16 16:47:18,322 [[MuleRuntime].uber.12: [solpubsub1dot2].pollinglistenercircuitbreaker.CPU_LITE @645c1d89] [processor: pollinglistenercircuitbreaker/processors/1; event: 3de73210-65f8-11ed-85fb-66bc5810864b] com.solace.connector.mulesoft.api.circuit.DefaultCircuitBreaker: Circuit tripping detected at failureCount=5
```
