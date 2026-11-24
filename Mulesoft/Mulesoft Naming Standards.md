# Mulesoft Naming Standards

Part of [[Mulesoft Development principles]].

Consistent names make flows easy to find in logs, Runtime Manager and code review. The first rule always wins: **match what the repository already does** before applying anything below.

## Projects and APIs

*Source: article.*

- Use a meaningful, structured project name that shows the API-led layer.
- Suffix by layer: `-s-api` (System), `-p-api` (Process), `-e-api` (Experience).

| Good | Avoid |
| --- | --- |
| `customer-s-api` | `cust_api1` |
| `order-p-api` | `api_final_v2` |
| `order-e-api` | |

## Flows and sub-flows

*Source: article, plus my own note on the plugin's client naming.*

- Article: use camelCase and a prefix or suffix that signals purpose, for example `processOrderFlow` or `errorHandlerSubFlow`.
- Plugin: reusable client sub-flows are named after the operation, in kebab-case, such as `erp-get-order` or `salesforce-upsert-account`.
- These two styles can coexist (camelCase for API flows, kebab-case for client operations), but pick one per project and keep it. Do not rename existing flows only to match this note.

## `flow-ref` display names

*Source: plugin.*

The `doc:name` of a `flow-ref` must exactly match the flow or sub-flow it calls. This keeps the canvas, search and stack traces consistent. It is one of the explicit checks in the `mulesoft-review` skill.

## Queues

*Source: article.*

- Use structured, dotted names such as `order.processing.queue`.
- Avoid generic names like `testQueue`.

## Notes and labels

*Source: article.*

- Give connectors and components a clear label so a reviewer can follow the flow without opening every element.
- Add a short note or comment on a flow explaining its purpose. Missing or generic descriptions defeat the point.

```xml
<flow name="validateOrderFlow">
    <!-- Validates the order payload before it is processed further -->
    <logger message="Validating order request" level="INFO"/>
</flow>
```

## Related

- [[Mulesoft Project Structure and Reuse]] for where named artifacts live
- [[Mulesoft DataWeave Conventions]] for variable and context-object names
