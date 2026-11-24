# Mulesoft DataWeave Conventions

Part of [[Mulesoft Development principles]]. *Sources: article (formatting and comments) and my mulesoft-dev-plugin (`design-conventions` reference).*

## Formatting and comments

*Source: article.*

- Indent properly, one field per line.
- Comment complex transformations so the intent is clear later.

```dataweave
%dw 2.0
output application/json
// Formatting order data before sending to the downstream API
var formattedDate = now() as String {format: "yyyy-MM-dd"}
---
{
    orderId: payload.id,
    customerName: payload.customer.name,
    orderDate: formattedDate
}
```

Avoid cramming the whole transformation onto one line: it hides the structure and is hard to review.

## Null safety with `default`

*Source: plugin.*

- Use `default` for parsing, optional fields, casts, indexing and other nullable paths, when it prevents a runtime error **and** there is a meaningful fallback.
- Do not use arbitrary defaults that hide a missing required field. A required field that is absent should fail validation, not be quietly replaced.

## Where the code lives

*Source: plugin.*

- Once a mapping grows past roughly 50 lines, move it into a named `.dwl` module or mapping in `src/main/resources/dw/`. Reference it from the Transform Message `resource`, or import it from a short inline script.
- Keep short, local transforms inline when that is clearer than a separate file.
- Reuse shared mappings and helper functions instead of copying them.
- Declare input and output MIME types when they matter to the transformation.

## Multiline logic in XML attributes

*Source: plugin.*

Never format DataWeave in an XML attribute with literal `\\n` sequences. Keep a simple expression on one line, or use a `.dwl` resource or a Transform Message CDATA block for multiline logic, then validate with Maven.

## Variables and payload state

*Source: plugin.*

- If two or more consecutive variables are independent, set them in one `ee:transform` with several `ee:set-variable` targets, not a chain of `set-variable` components.
- Keep values for one concern in a single purpose-named object variable, for example `vars.dbConfig = { tableName, query, inputParameters }` instead of three separate variables.
- Name context objects for their purpose (`httpRequest`, `dbConfig`, `orderLookup`). Avoid `data` and `temp`.
- Make payload contracts explicit at flow boundaries. Do not rely on a payload being changed incidentally inside another flow.

## Related

- [[Mulesoft Naming Standards]]
- [[Mulesoft Project Structure and Reuse]]
- [[Mulesoft MUnit Testing]]: assert on mapped business values
