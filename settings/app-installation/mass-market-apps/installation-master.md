# Installation Wizard for Mass-Market Application

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The installation wizard is suitable for mass-market applications with their own interface that require one-time setup before the first launch. The wizard can display a form, retain settings, register event handlers and placements, or prepare other objects in Bitrix24. If the application has no interface, consider an [installation callback](./installation-callback.md) or a [setup wizard for REST-only applications](./rest-only-installation-master.md).

## Installation Wizard Requirements

To use an installation wizard:

- the installation must be performed by a Bitrix24 administrator or a user who is allowed to install applications
- in the developer account, specify the public wizard URL in the *Installation application URL* field of the application version. For a production version, use HTTPS and do not specify `localhost`, a local network address, or an address with a self-signed SSL certificate
- connect the Bitrix24 JS library on the wizard page so that you can call [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) after successful setup
- if the wizard makes REST API requests, the application needs the corresponding permissions

Do not pass tokens or secrets in the wizard URL. Retain settings and authorization data on the application side.

If the application retains settings for different Bitrix24 accounts, associate each record on your side with `member_id`. This identifier does not change when the Bitrix24 address changes.

## Where to Specify the Wizard URL

Specify the wizard URL in the application version in the developer account. The field depends on the selected installation scenario:

- if the application has an interface, select the installation wizard scenario and specify the wizard URL in the *Installation application URL* field
- if the application has no interface, select the [installation callback](./installation-callback.md) and specify the installation handler URL

For an application with an interface, also specify its main URL in the *Application URL* field. After the wizard completes successfully, Bitrix24 opens this main URL.

## How the Installation Wizard Works

When the application is installed for the first time, Bitrix24 opens a slider and displays the URL from the *Installation application URL* field in a frame.

The page at this URL serves as the installation wizard. It can contain a settings form, an information screen, or an external service connection check.

The same URL can initialize and create the required objects in Bitrix24. For example, it can:

- set up [event handlers](../../../api-reference/events/index.md)
- register [widgets](../../../api-reference/widgets/index.md) in the required placements
- add a [payment system provider](../../../api-reference/pay-system/index.md)
- register an [SMS messaging provider](../../../api-reference/messageservice/index.md)

Bitrix24 considers the installation complete only after [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) is called. Until then, each time an administrator or a user with installation permission opens the application, Bitrix24 displays the installation URL. Other users see a message that the application is not installed.

Application events and placements are unavailable until installation is complete, even if [event.bind](../../../api-reference/events/event-bind.md) and [placement.bind](../../../api-reference/widgets/placement-bind.md) completed successfully.

After `BX24.installFinish()` is called, Bitrix24 opens the main URL from the *Application URL* field. Call this method last, after retaining settings, registering widgets, and subscribing to events.

```html
<script src="//api.bitrix24.com/api/v1"></script>
<script>
    function callMethod(method, params) {
        return new Promise(function(resolve, reject) {
            BX24.callMethod(method, params, function(result) {
                if (result.error()) {
                    reject(result.error());
                    return;
                }

                resolve(result.data());
            });
        });
    }

    BX24.init(async function() {
        try {
            await callMethod('placement.bind', {
                PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
                HANDLER: 'https://example.com/deal-tab',
                TITLE: 'Application Data'
            });

            await callMethod('event.bind', {
                event: 'ONCRMDEALADD',
                handler: 'https://example.com/event-handler'
            });

            BX24.installFinish();
        } catch (error) {
            console.error('Installation error:', error);
        }
    });
</script>
```

You can check the installation status using [app.info](../../../api-reference/common/system/app-info.md). A value of `false` in the `INSTALLED` field means the installation is not complete.

## When to Choose Another Installation Option

#|
|| **Option** | **When to Choose It** | **What to Configure** ||
|| [Installation Callback](./installation-callback.md) | The application has no interface and needs to receive OAuth tokens and work in event handlers | URL in the *Installation event handler URL* field ||
|| [Setup Wizard for REST-only Applications](./rest-only-installation-master.md) | The application has no interface but needs a simple Bitrix24-style settings form | `URL_SETTINGS` parameter in the *Application settings* field ||
|| [Add Without an Installation Scenario](./index.md) | The application does not need one-time installation actions | No additional installation setup is required ||
|#

## Features of Installing a Static Application

With a static application, you upload an archive of the entire solution instead of specifying paths to your own server. Therefore, Bitrix24 uses the `install.html` file from the solution archive as the installation wizard URL.

The workflow remains the same: use JS functions in `install.html` to make the required requests, and call [BX24.installFinish()](../../../sdk/bx24-js-sdk/system-functions/bx24-install-finish.md) after the wizard completes successfully.

If there is no `install.html` file in the archive root, Bitrix24 assumes that the installation wizard is not required and immediately opens `index.html` from the same archive.
