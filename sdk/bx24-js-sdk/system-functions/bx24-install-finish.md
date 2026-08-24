# Notify about the completion of the installer BX24.installFinish

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

```js
BX24.installFinish(): void;
```

The function `BX24.installFinish` indicates the completion of the installer or application setup.

If the function is called during the installer startup phase, the page will reload and the application will launch. If called during the setup phase, it will trigger the handlers for [BX24.init](./bx24-init.md). In other cases, there will be no effect from the call.

{% note warning "" %}

Until `installFinish` is called, an application with an interface stays half-installed, and the portal does not deliver events to it: chat-bot handlers, handlers registered with [event.bind](../../../api-reference/events/event-bind.md), and other outgoing calls never reach your endpoints. Placements registered with [placement.bind](../../../api-reference/widgets/placement-bind.md) will not appear in the interface either.

Nothing fails loudly. Registration succeeds, the methods return a result, the handlers are listed when queried — and no traffic arrives. Every visible signal says the setup is correct, which is why this is usually debugged at the wrong layer: nginx, tunnels, TLS, routing.

Call `installFinish` once, from the application setup page, as the last step of the install flow — after you have registered the placements and subscribed to the events.

To check an application after the fact, use [app.info](../../../api-reference/common/system/app-info.md): the `INSTALLED` field set to `false` means `installFinish` was never executed.

{% endnote %}

An application without an interface that uses only the API does not need the call: its installation completes automatically, and the method itself works only inside the application interface frame in the browser. Details are in [{#T}](../../../settings/app-installation/installation-finish.md).

No parameters.

## Example

```js
document.addEventListener("DOMContentLoaded", function() {
    BX24.init(function(){
        BX24.installFinish();
    });
});
```

## Continue your study

- [{#T}](./bx24-init.md)
- [{#T}](./bx24-install.md)
- [{#T}](./bx24-get-auth.md)
- [{#T}](./bx24-refresh-auth.md)
- [{#T}](../../../settings/app-installation/installation-finish.md)