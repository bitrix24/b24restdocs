# Installation Wizard for Local Application

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A local application needs an installation wizard if initial setup is required before use. The wizard can display a form, retain settings, register event handlers and widgets, or prepare other objects in Bitrix24.

For a server-side application, the wizard opens if the *Initial installation path* field is filled in and the *Application completes installation itself* option is disabled. If you enable this option, Bitrix24 sends the installation data to the specified URL in the [`ONAPPINSTALL`](../../../api-reference/common/events/on-app-install.md) event and immediately marks the application as installed. This scenario is described in [Installation Callback](./installation-callback.md).

## When the Installation Wizard Opens

After a server-side local application is added, the first time it is opened, Bitrix24 displays a slider with the URL from the *Initial installation path* field. The installation page is available to an administrator or a user with permission to install the application. Other users see a message that the application is not installed.

The page at this URL serves as the installation wizard. It can perform one-time actions:

- display an application settings form
- retain application settings on your server
- register [event handlers](../../../api-reference/events/index.md)
- register [widgets](../../../api-reference/widgets/index.md) in the required placements
- add a [payment system provider](../../../api-reference/pay-system/index.md)
- register an [SMS messaging provider](../../../api-reference/messageservice/index.md)

## Where to Configure the Installation Wizard

In Bitrix24, open *Applications > Developer resources > Other > Local application* and select the application type.

### Server-side Application

Select *Server-side* and fill in the *Initial installation path* field.

Specify the public URL of the installation page on your server, for example, `https://example.com/install.php`. The file can have any name. You can specify an HTTP or HTTPS URL, but use HTTPS for a production application.

Specify the application's main URL separately in the *Handler path* field. After installation is complete, Bitrix24 opens this URL instead of the wizard page.

If the *Initial installation path* field is empty, Bitrix24 immediately considers the local application installed and does not open the wizard.

Do not enable *Application completes installation itself* if the specified URL must open a wizard page. When the option is enabled, this URL is used as the installation callback, and the application immediately receives the installed status.

### Static Application

Select *Static* and upload the application ZIP archive. The *Initial installation path* and *Handler path* fields are not displayed for this type.

Bitrix24 uses the `install.html` file from the archive root as the installation wizard. If this file is absent, the wizard does not open: Bitrix24 immediately opens `index.html` from the same archive.

## Requirements and Permissions

- the user must have permission to install local applications
- for a static application, `install.html` must be located in the ZIP archive root
- the application form must include the permissions required for installation REST API calls
- the server-side application wizard URL must be accessible over the network

Do not place secrets or tokens in the HTML or other client-side code of the installation page. For a server-side application, retain them in secure storage on your side.

## How to Complete Installation

The wizard must explicitly notify Bitrix24 that installation completed successfully. To do this, call the [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) JS method on the server-side application installation page or in the static application's `install.html` file.

Call `BX24.installFinish()` as the last step, after retaining settings, registering event handlers and widgets, and completing other required operations. Until the method is called, Bitrix24 considers the application not installed. When a user with installation permission opens the server-side application again, Bitrix24 opens the URL from *Initial installation path*. For a static application, it opens `install.html`. Other users see a message that installation is incomplete.

Until installation is complete, events are not delivered and placements do not appear in the interface, even if [event.bind](../../../api-reference/events/event-bind.md) and [placement.bind](../../../api-reference/widgets/placement-bind.md) completed successfully.

You can check the installation status using [app.info](../../../api-reference/common/system/app-info.md). A value of `false` in the `INSTALLED` field means installation is not complete.

Minimal wizard page example:

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Application Installation</title>
    <script src="//api.bitrix24.com/api/v1"></script>
</head>
<body>
    <button id="finish-install">Complete Installation</button>

    <script>
        BX24.init(function() {
            document.getElementById("finish-install").addEventListener("click", function() {
                // Perform the required installation actions here.
                BX24.installFinish();
            });
        });
    </script>
</body>
</html>
```

## Continue Exploring

- [{#T}](../installation-finish.md)
- [{#T}](./installation-callback.md)
- [{#T}](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md)
