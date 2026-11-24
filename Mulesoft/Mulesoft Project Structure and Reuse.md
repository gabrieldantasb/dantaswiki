# Mulesoft Project Structure and Reuse

Part of [[Mulesoft Development principles]]. *Source: my mulesoft-dev-plugin (`mulesoft-development` skill), unless noted.*

## Before creating anything

- Search `src/main/mule` and `src/main/resources/dw` for an equivalent flow, sub-flow, client operation, mapping or property. Reuse it when it fits.
- Keep existing naming, connector versions, project structure and error-handling conventions unless the change is meant to alter them.
- Read only the files relevant to the change.

## Default layout

Use this unless the repository already has a compatible convention:

```text
src/main/mule/global.xml                  # Connector configuration objects only
src/main/mule/clients/<system>-client.xml # Reusable external-system sub-flows
src/main/mule/<domain>/                   # Domain or API orchestration flows
src/main/resources/config/                # Environment and shared properties
src/main/resources/dw/                    # Reusable DataWeave modules and mappings
```

## `global.xml` holds configuration only

Declare connector configurations (`http:request-config`, `db:config`, `sfdc:config`, `ftp:config` and similar) only in `global.xml`. Client files contain operations and sub-flows and reference those configurations.

## Thin orchestration flows

An orchestration flow should do four things:

1. validate input
2. assemble context
3. call reusable business or client sub-flows
4. map the response

Anything reused in more than one place moves out: a sub-flow, a DataWeave module, or a shared mapping, whichever fits.

## Client sub-flows

Each external system gets one `clients/<system>-client.xml` with a focused sub-flow per operation (`erp-get-order`, `salesforce-upsert-account`).

- The caller supplies the dynamic inputs: path, query parameters, headers, body, SQL, input parameters. Document the context object or variables the sub-flow expects.
- Credentials and endpoints stay property-driven and live in `global.xml`.
- Invoke with `flow-ref`; the `doc:name` must match the target name (see [[Mulesoft Naming Standards]]).
- Log at the boundary, safely: see [[Mulesoft Logging and Correlation IDs]].

## Shared values

- Move a static value to `src/main/resources/config/` when it is used in more than one place.
- Do not externalize a one-off value just to satisfy the rule.

See [[Mulesoft Configuration and Secrets]] for what belongs in properties and what must stay out of XML entirely.

## Pre-handoff check

Look at the changed XML and DataWeave for:

- duplicated logic
- repeated hard-coded values
- connector configurations outside `global.xml`
- `flow-ref` display names that differ from their targets

Then run the repository's existing validation or Maven command if there is one, and say what could not be run. If a `.raml` file changed, also see [[Mulesoft RAML Validation]].
