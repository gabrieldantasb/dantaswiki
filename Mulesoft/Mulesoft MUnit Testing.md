# Mulesoft MUnit Testing

Part of [[Mulesoft Development principles]]. *Source: article (section 6), plus a scope note from my mulesoft-dev-plugin.*

## Assert business outcomes

Test what the flow is supposed to produce, not just that something came back.

```xml
<munit-tools:assert-that actual="#[payload.status]" is="#[MunitTools::equalTo('success')]"/>
```

Avoid generic assertions such as `assert-not-null` on the whole payload. They pass for almost any result and prove nothing about correctness.

## Cover cases, not just coverage

Prioritize edge cases, error conditions and input variations over chasing a coverage percentage. Include the error paths from [[Mulesoft Error Handling]], for example that a bad input produces the intended failure.

## Mock by `doc:id`

When mocking a connector call, match on `doc:id` only, not on the label (`doc:name`) or the config name. Labels and config names get renamed during normal clean-up (see [[Mulesoft Naming Standards]]), and matching on them forces you to re-mock every test.

```xml
<munit-tools:mock-when processor="http:request">
    <munit-tools:with-attributes>
        <munit-tools:with-attribute attributeName="doc:id" whereValue="a1b2c3-your-doc-id"/>
    </munit-tools:with-attributes>
    <munit-tools:then-return>
        <munit-tools:payload value="#[readUrl('classpath://mocks/order.json')]"/>
    </munit-tools:then-return>
</munit-tools:mock-when>
```

The `doc:id` above is a placeholder. Use the real ID from the component in your flow.

## Scope in my plugin

My `mulesoft-development` and `mulesoft-review` skills deliberately **do not create or modify MUnit tests** unless I explicitly ask. Treat tests as a separate, requested piece of work.

## Related

- [[Mulesoft DataWeave Conventions]]: mappings are easiest to test when extracted to `.dwl` modules
- [[Mulesoft Project Structure and Reuse]]
