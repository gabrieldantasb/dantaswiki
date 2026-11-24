# Mulesoft Logging and Correlation IDs

Part of [[Mulesoft Development principles]]. *Sources: article (logging, transaction tracking) and my mulesoft-dev-plugin (client boundary logging).*

## Log levels

*Source: article.*

| Level | Use for | Example |
| --- | --- | --- |
| `INFO` | Business events and high-level flow tracking | "Order received for Customer ID 12345" |
| `DEBUG` | Detailed data for troubleshooting | "Calling external API with payload ..." |
| `ERROR` | Failures and exceptions | "Failed to connect to Database: TimeoutException" |

```xml
<logger message="Order received. ID: #[payload.orderId]" level="INFO"/>
<logger message="Sending request payload: #[payload]" level="DEBUG"/>
<logger message="Exception occurred: #[error.description]" level="ERROR"/>
```

## What never to log

*Source: plugin.*

Never log secrets, authorization headers, tokens, passwords or sensitive payload fields. This applies to every level.

> My note: the article's `DEBUG` example logs the full payload. That is fine for non-sensitive data, but a full-payload debug log can leak personal or credential data if someone turns the level up in production. Log selected safe fields when in doubt.

## Logging at client boundaries

*Source: plugin.*

In each external-system client sub-flow (see [[Mulesoft Project Structure and Reuse]]), log:

- the operation name
- safe parameters only
- a status or result summary
- the correlation ID

## Transaction tracking with correlation IDs

*Source: article, section 5.*

A correlation ID ties together every log line and message belonging to one transaction across the Experience, Process and System APIs.

### API-led (synchronous)

Take the incoming correlation ID, or generate one if it is missing, then log it in every request and response.

```xml
<set-variable variableName="correlationId"
    value="#[attributes.headers.correlationId default (randomUUID())]"/>
<logger message="Transaction ID: #[vars.correlationId]" level="INFO"/>
```

Pass it on in the headers of every downstream call.

### Queue-based (asynchronous)

Send the ID with the message as a user property, then read and log it on the consumer side.

```xml
<amq:publish config-ref="AMQ_Config" destination="queue://yourQueueName">
    <amq:user-property key="correlationId" value="#[vars.correlationId]"/>
</amq:publish>
```

> My note: Mule 4 already exposes a built-in `correlationId`, and the HTTP connector carries it as the `X-Correlation-ID` header. An explicit variable is still useful for a custom header name or to carry the ID across a queue. Check the exact behaviour on the runtime version in use before relying on it.

## Related

- [[Mulesoft Error Handling]]: include the correlation ID in error logs
- [[Mulesoft Alerts and Monitoring]]: alerts are far more useful when they carry the ID
