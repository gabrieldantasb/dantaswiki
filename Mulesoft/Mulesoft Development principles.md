# Mulesoft Development principles

Working rules for building Mule 4 applications: readable, reusable, safe to operate. This note is the entry point; each topic has its own note.

## Where these principles come from

- **Article:** [The Ultimate Guide to MuleSoft Coding Standards and Development Principles](https://medium.com/@ranjithreddy518/the-ultimate-guide-to-mulesoft-coding-standards-and-development-principles-f5d8407c3f23) (RanjithReddyVadiyala, Mar 2025). It covers naming, logging, DataWeave style, configuration, transaction tracking, MUnit, error handling and alerts.
- **My own harness:** [mulesoft-dev-plugin](https://github.com/gabrieldantasb/mulesoft-dev-plugin), a Claude Code / Codex plugin with a development skill, a review skill and a RAML validation gate. It adds the layout, reuse and client-boundary rules.

Each topic note marks which rules come from which source. Where I added commentary of my own, it is labelled as such.

## Topics

| Topic | What it covers |
| --- | --- |
| [[Mulesoft Naming Standards]] | Projects, flows, queues, notes and labels, `flow-ref` names |
| [[Mulesoft Project Structure and Reuse]] | File layout, `global.xml`, client sub-flows, thin orchestration flows |
| [[Mulesoft Error Handling]] | `on-error-propagate` vs `on-error-continue`, sync vs async APIs |
| [[Mulesoft Logging and Correlation IDs]] | Log levels, what never to log, transaction tracking |
| [[Mulesoft DataWeave Conventions]] | Formatting, comments, `default`, extracting `.dwl` modules, variables |
| [[Mulesoft Configuration and Secrets]] | Properties vs YAML, environment values, secure properties |
| [[Mulesoft MUnit Testing]] | What to assert, what to mock, coverage |
| [[Mulesoft Alerts and Monitoring]] | Which errors page someone, avoiding alert fatigue |
| [[Mulesoft RAML Validation]] | The AMF validation gate and the plugin that enforces it |

## The principles in one list

1. Follow the repository's existing conventions before introducing new ones.
2. Search for an existing flow, sub-flow, mapping or property before creating one.
3. Keep orchestration flows thin; put reusable logic in sub-flows, DataWeave modules and client files.
4. Connector configurations live in one place (`global.xml`).
5. Nothing environment-specific or secret is hard-coded in Mule XML.
6. Every request carries a correlation ID and every log line can be traced back to it.
7. Handle errors deliberately: propagate when the caller must know, continue only when it is safe to.
8. Never log secrets, tokens or sensitive payload fields.
9. Test business outcomes, not "payload is not null".
10. Alert on what needs action; log the rest.
