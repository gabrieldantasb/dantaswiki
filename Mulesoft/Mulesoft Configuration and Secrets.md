# Mulesoft Configuration and Secrets

Part of [[Mulesoft Development principles]]. *Sources: article (externalizing configuration) and my mulesoft-dev-plugin (placement and secrets).*

## Principle

Keep environment-specific, secret and deploy-time values out of Mule XML. Put them in property files and, for secrets, use the repository's approved secure-property mechanism (for example Mule's Secure Properties module). Files live under `src/main/resources/config/`, see [[Mulesoft Project Structure and Reuse]].

## Properties file vs YAML

*Source: article.*

| Use | When |
| --- | --- |
| `.properties` | Simple key-value settings: URLs, timeouts |
| `.yaml` | Structured or nested configuration, lists, several systems |

### Properties

```properties
order.api.url=https://api.example.com/orders
order.api.timeout=5000
```

```xml
<http:request url="${order.api.url}" timeout="${order.api.timeout}"/>
```

### YAML

```yaml
orderApi:
  url: https://api.example.com/orders
  timeout: 5000
  retries: 3
```

```xml
<http:request url="${orderApi.url}" timeout="${orderApi.timeout}"/>
```

YAML reads better, supports nesting and avoids the repetition of long dotted keys. Properties stay fine when the configuration is flat and small.

## Where the article needs care

*My notes.* The article's examples put plain-text `password=secret` values in the properties and YAML files, and build a SQL string by inserting a property value into the query. Do not copy that:

- Never commit real or plain-text credentials. Use secure (encrypted) properties or environment-provided secrets.
- Pass SQL values as input parameters, not by concatenating property values into the query text.
- This vault's own [[README]] makes the same point about not storing secrets in notes.

## Shared values

A static value used in more than one place moves to `src/main/resources/config/`. A one-off value does not need to be externalized just for the sake of it.

## Related

- [[Mulesoft Logging and Correlation IDs]]: never log secret values
- [[Mulesoft Naming Standards]]
