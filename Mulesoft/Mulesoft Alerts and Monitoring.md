# Mulesoft Alerts and Monitoring

Part of [[Mulesoft Development principles]]. *Source: article (section 8).*

## Purpose

Alerts give real-time visibility into production health, so the team can fix a problem before users feel it.

## Write useful error messages

- Log and report the detailed error description, not a generic message.
- Include the correlation ID so the failing transaction can be traced: see [[Mulesoft Logging and Correlation IDs]].
- Never put secrets or sensitive payload fields into an alert: see [[Mulesoft Configuration and Secrets]].

## What to alert on

- Server errors (5xx) and timeouts
- Other significant failures that need action

## Avoid alert fatigue

- Alert on what a person must act on. Everything else belongs in logs.
- Make sure each alert reaches the people who can resolve it.
- Revisit noisy alerts instead of muting the channel.

## Tooling

The article suggests Splunk or MuleSoft's built-in monitoring (Anypoint Monitoring) for tracking errors and learning from them over time.

## Related

- [[Mulesoft Error Handling]]: async failures are often only visible through alerts and queues
