# Notify Completion of Installer Work BX24.installFinish

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.installFinish(): void;
```

The `BX24.installFinish` function reports that the installer or the application setup has finished. With this call, an application with an interface completes its installation in Bitrix24.

The function works only inside the frame of an [application](../../../settings/app-installation/index.md) — it cannot be called from a server-side installation handler. How to connect the library is described in the [BX24.js Library Overview](../index.md).

Call the function after the library has received the Bitrix24 data: called earlier, it completes the installation prematurely at the installer stage — see [Error Handling](#errors) for details.

The function does not need a scope of its own: it does not call the REST API. Permissions and scopes are checked in the Bitrix24 methods that the installation flow itself calls: for example, [placement.bind](../../../api-reference/widgets/placement-bind.md) requires administrator rights, while [event.bind](../../../api-reference/events/event-bind.md) is available to any user — with limitations for offline events and a foreign `auth_type`.

## Function Parameters

No parameters.

## Behavior at Different Installation Stages {#stages}

The result depends on the stage at which the function is called.

#|
|| **Stage** | **When It Occurs** | **What Happens on the Call** ||
|| Installer | The application is not installed yet, an installation page URL is specified for it, and the current user has the right to install applications. Bitrix24 opens the installation page in the frame | Bitrix24 marks the application as installed and reloads the Bitrix24 page on which the application is open ||
|| Setup | The first-launch handler registered with [BX24.install](./bx24-install.md) is running | Control passes to the next `BX24.install` handler. After the last handler, Bitrix24 records that the application has already been launched for this user and runs the [BX24.init](./bx24-init.md) handlers ||
|| Regular launch | The application is already installed and opened by the user | The call does nothing and does not report it ||
|#

An application without an interface that uses only the API does not need the `installFinish` call: its installation completes automatically. Details are in [{#T}](../../../settings/app-installation/installation-finish.md).

{% note warning "" %}

Until `installFinish` is called, an application with an interface is considered not installed, and Bitrix24 does not deliver events to it: chat-bot handlers and handlers registered with [event.bind](../../../api-reference/events/event-bind.md) receive no requests. Placements registered with [placement.bind](../../../api-reference/widgets/placement-bind.md) will not appear in the interface.

Nothing fails loudly: registration succeeds, the handlers are listed when queried, but no traffic arrives.

Call `installFinish` as the last step of the installation flow — after you have registered the placements and subscribed to the events. If the registration failed, do not complete the installation at the installer stage. Inside the [BX24.install](./bx24-install.md) handler the rule is the opposite: call `installFinish` in any case, otherwise the handler chain will stop.

{% endnote %}

## Code Example

{% include [Note on examples](../../../_includes/examples.md) %}

The code is placed on the page that is specified as the application's installation URL. The installation flow registers a placement and an event subscription with the [BX24.callBatch](../how-to-call-rest-methods/bx24-call-batch.md) batch and completes the installation only after both registrations have succeeded.

```html
<div id="install-error"></div>

<script src="//api.bitrix24.com/api/v1/"></script>
<script>
    // wait for the Bitrix24 data: installFinish called earlier marks the application as installed without checking the stage
    BX24.init(function() {
        BX24.callBatch({
            bindPlacement: {
                method: 'placement.bind',
                params: {
                    PLACEMENT: 'CRM_DEAL_DETAIL_TAB',
                    HANDLER: 'https://example.com/b24/deal-tab.php',
                    TITLE: 'Deal Analytics'
                }
            },
            bindEvent: {
                method: 'event.bind',
                params: {
                    event: 'ONCRMDEALUPDATE',
                    handler: 'https://example.com/b24/deal-update.php'
                }
            }
        }, function(result) {
            if (result.bindPlacement.error() || result.bindEvent.error()) {
                // do not complete the installation: without the placement and the subscription the application will not work
                console.error('Registration error: ', result.bindPlacement.error(), result.bindEvent.error());
                document.getElementById('install-error').textContent = 'Installation is not complete, please try again';
                return;
            }

            // installFinish comes last, when the placements and events are already registered
            BX24.installFinish();
        });
    });
</script>
```

## Response Handling

The function returns no data (`void`) and does not accept a handler. The result is indicated by the behavior of Bitrix24 — different at each stage, as the table [Behavior at Different Installation Stages](#stages) describes.

Call the function once per stage. At the installer stage, control passes to Bitrix24 after the call, the page reloads, and the code after `installFinish` may not run.

After the chain of [BX24.install](./bx24-install.md) handlers, the function becomes empty: the following calls, including those from the [BX24.init](./bx24-init.md) handlers, do nothing. Complete the setup inside the chain, not after it.

The installation status can be checked programmatically with the [app.info](../../../api-reference/common/system/app-info.md) method. The `result.INSTALLED` field has the [`boolean`](../../../api-reference/data-types.md) type. A fragment of the response for a completed installation:

```json
{
    "result": {
        "INSTALLED": true
    }
}
```

For an application with an interface, the value `false` means that `installFinish` has not been called yet. The other response fields are described on the page of the [app.info](../../../api-reference/common/system/app-info.md) method.

## Error Handling {#errors}

The function has no error codes of its own: it does not call the REST API but sends a message to the Bitrix24 page. Failures look like the absence of a result.

#|
|| **Situation** | **What Happens** | **What to Do** ||
|| The function is called after initialization, outside the installer stage and outside a `BX24.install` handler | The library replaces the function with an empty one, and the call does nothing | Call the function from the application installation page or from a [BX24.install](./bx24-install.md) handler ||
|| The application is installed by a server-side handler rather than by an installation page | There is nowhere to call the function from: there is no code in the frame | Complete the installation as described in [Installing an Application via a Callback Handler](../../../settings/app-installation/local-apps/installation-callback.md) ||
|| The function is called before the library initialization has finished | The message goes to Bitrix24 right away, bypassing the stage check: at the installer stage the application is marked as installed and the page reloads, even if the installation flow has not finished | Call the function in a [BX24.init](./bx24-init.md) handler or in a [BX24.install](./bx24-install.md) handler ||
|| The page is opened outside the application frame | The library does not initialize: on load it throws the exception `Unable to initialize Bitrix24 JS library!`, and the `BX24` object becomes `null` | Open the page as a Bitrix24 application ||
|| The user does not have the right to install applications | The application remains not installed. If the user opens an application that has an installation page specified, Bitrix24 shows them a message about the incomplete installation instead of the application interface | Complete the installation under an administrator account or an account with the right to install applications ||
|#

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)
- [{#T}](../../../settings/app-installation/installation-finish.md)
- [{#T}](../../../settings/app-installation/local-apps/installation-master.md)
