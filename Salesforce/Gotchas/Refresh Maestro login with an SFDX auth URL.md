# Refresh Maestro login with an SFDX auth URL

## Gotcha

An org can show a login error in Maestro and appear in red. Updating its Salesforce login record with an SFDX auth URL and refreshing Maestro has resolved this issue.

## Fix

1. In the VS Code terminal, use Salesforce CLI to show the auth URL for an org already authorized locally via a web-based OAuth login. Replace `<org-alias>` with the org's local alias or username:

   ```sh
   sf org auth show-sfdx-auth-url --target-org "<org-alias>" --no-prompt --json
   ```

2. Copy the value of `result.sfdxAuthUrl` from the JSON output into the SFDX auth URL field on the corresponding Salesforce login record, then save it.
3. Click **Refresh** in Maestro and try logging in again.
4. Check that **Has Login Error** on the Salesforce login record resets to `false` and that the org is no longer marked in red in Maestro.

Both `--no-prompt` and `--json` are optional: the first skips the confirmation prompt, while the second produces JSON and also skips the prompt. The SFDX auth URL is available for orgs authorized through a web-based OAuth flow, not the JWT bearer flow. See the [Salesforce CLI command documentation](https://github.com/salesforcecli/plugin-org#sf-org-auth-show-sfdx-auth-url) and [Salesforce authorization URL documentation](https://developer.salesforce.com/docs/platform/sfdx-dev/guide/sfdx-dev-auth-url.html).

> [!warning] Treat the auth URL as a credential
> It contains authentication material, including a refresh token. Only enter it in the intended login record; do not paste it into notes, commit it to Git, or share it in messages.
