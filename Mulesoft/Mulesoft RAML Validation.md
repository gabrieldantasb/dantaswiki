# Mulesoft RAML Validation

Part of [[Mulesoft Development principles]]. *Source: my [mulesoft-dev-plugin](https://github.com/gabrieldantasb/mulesoft-dev-plugin).*

## Rule

Any creation, edit, rename, move or deletion of a `.raml` file is a validation-affecting change, even when it happens inside a larger MuleSoft task. It is a completion gate, not a formatter, and it is not optional.

## How it works

The plugin ships a shared validator in `tools/raml-validator`, built on the official `amf-client-js` (AMF) package. It lives in the plugin, not in the Mule project.

- It validates every discoverable `#%RAML 1.0` API entry point affected by the change.
- It follows local `uses` and `!include` references relative to the file that declares them.
- A library, trait, resource type, data type or include is **not** assumed to be an API root.
- Parse errors, unresolved references and AMF validation errors return a nonzero exit code with file, location and message.

```powershell
node tools/raml-validator/validate-raml.cjs 'C:\path with spaces\api.raml'
node tools/raml-validator/validate-affected-raml.cjs --project 'C:\path with spaces\mule-api' --changed 'api\libraries\common.raml'
```

Install its dependencies once with `npm install --prefix tools/raml-validator`.

## Workflow

1. Identify the changed RAML files and the affected main API entry points.
2. Run the validator for those entry points.
3. If it fails, read the AMF diagnostics, fix what the change introduced, and re-run. Any later RAML change invalidates a previous pass.
4. If no entry point can be found or the validator cannot run, report that as a blocker. Do not claim the task is complete.

## Stop hook

`hooks/hooks.json` runs a Stop hook after an agent turn. In a Git worktree it detects uncommitted RAML changes, finds the affected entry points, and blocks completion until AMF passes. It only reads source files and never writes application files. Codex requires the user to review and trust plugin hooks first; Claude Code runs them when the plugin is enabled.

## Related

- [[Mulesoft Project Structure and Reuse]]: the pre-handoff check
