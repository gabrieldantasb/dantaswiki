# Anypoint Studio Exchange SSO login

## Gotcha

Permission issues can occur when signing in to Anypoint Exchange from Anypoint Studio through SSO. Configuring Studio to use an organization-authorized external browser resolved the issue.

## Fix

1. Open **Preferences** in Anypoint Studio and go to **General → Web Browser**.
2. Select **Use external web browser**, then select **Chrome** or another browser authorized by your organization. Click **Apply**.
3. In the same preferences window, go to **Anypoint Studio → Browser**.
4. Set **Default Browser** to **Native**, then apply the setting.
5. Try signing in to Anypoint Exchange through SSO again.

The screenshot shows the external browser setting with Chrome selected:

![[Mulesoft/Gotchas/Attachments/Anypoint Studio external browser preferences.png]]

For other MuleSoft notes, see [[Mulesoft Development principles]].
