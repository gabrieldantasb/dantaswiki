# Mulesoft Error Handling

Part of [[Mulesoft Development principles]]. *Source: article (section 7), with a few clarifications of my own marked as such.*

## The two handlers

| Handler | Behaviour | Use it when |
| --- | --- | --- |
| `on-error-propagate` | Handles the error, then **re-throws** it. Processing stops and the caller sees the failure. | The caller must be told: synchronous APIs returning 400, 404, 500 and similar. |
| `on-error-continue` | Handles the error **locally** and the flow carries on as if it had succeeded. | The failure is non-critical: log it and use a default or an alternative path. |

Rule of thumb: if the failure should stop the transaction, propagate. If the flow can legitimately recover, continue.

> My note: with `on-error-continue`, the caller of a sync flow gets a normal (success) response built from whatever the handler leaves in the payload. If the caller needs an error status, either propagate, or set the response status explicitly in the handler. Check this against the Mule runtime version in use.

## Sync APIs (request-response)

- Map errors to meaningful HTTP status codes: 404 for not found, 400 for invalid input, 500 for unexpected failures.
- The client must be able to tell what went wrong without reading server logs.
- Use `on-error-propagate` (or a handler that sets the status) so the status code reaches the caller.

## Async APIs (event-driven, long-running)

- The immediate response is usually `202 Accepted`; failures happen later.
- Log errors so the root cause can be found quickly, even though nobody is waiting on a response.
- Track failures through queues (retry or dead-letter) or an external monitoring system.
- See [[Mulesoft Alerts and Monitoring]] for what should notify a person.

## Custom strategies

Choose the handling per use case:

- **Retry** for temporary failures such as timeouts or connectivity blips.
- **Alert** for critical failures that need a human.
- **Default or fallback value** for optional enrichment calls.

## Practical rules

- Always log a detailed error description (`error.description`, error type), never a generic "something went wrong". Include the correlation ID, see [[Mulesoft Logging and Correlation IDs]].
- Don't swallow errors silently. Every handler should either re-throw, return a deliberate response, or log why it continued.
- Put shared handling in one reusable place (for example a common error handler flow) instead of copying it into each flow. Related: [[Mulesoft Project Structure and Reuse]].
- Test the error paths, not only the happy path: see [[Mulesoft MUnit Testing]].
- Do not leak secrets or sensitive payload fields in error responses or logs.
