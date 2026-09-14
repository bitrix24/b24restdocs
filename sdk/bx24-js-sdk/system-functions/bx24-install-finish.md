# Notify about the completion of the installer BX24.installFinish

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

```js
BX24.installFinish(): void;
```

The function `BX24.installFinish` indicates the completion of the installer or application setup.

If the function is called during the installer startup phase, the page will reload and the application will launch. If called during the setup phase, it will trigger the handlers for [BX24.init](./bx24-init.md). In other cases, there will be no effect from the call.

An application without an interface that uses only the API does not need the call: its installation completes automatically. Details are in [{#T}](../../../settings/app-installation/installation-finish.md).

No parameters.

{% note warning "" %}

Until `installFinish` is called, an application with an interface is considered not installed, and Bitrix24 does not deliver events to it: chat-bot handlers and handlers registered with [event.bind](../../../api-reference/events/event-bind.md) receive no requests. Placements registered with [placement.bind](../../../api-reference/widgets/placement-bind.md) will not appear in the interface.

Nothing fails loudly: registration succeeds, the handlers are listed when queried, but no traffic arrives.

Call `installFinish` as the last step of the install flow — after you have registered the placements and subscribed to the events.

To check the installation status, use [app.info](../../../api-reference/common/system/app-info.md): the value `false` in the `INSTALLED` field means `installFinish` was never executed.

{% endnote %}

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
