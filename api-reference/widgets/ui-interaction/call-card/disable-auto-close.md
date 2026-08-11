# Disable Auto-Close disableAutoClose

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../../scopes/permissions.md) — registration of the placement, [`telephony`](../../../scopes/permissions.md) — access to the call card placement
>
> Who can execute the command: any user

The `disableAutoClose` command disables the automatic closing of the call card and resets the delayed-closing timer to 65 seconds if it has already started.

{% note info "" %}

The command operates within the application context in the `CALL_CARD` placement. This is a JS interface command, not a REST method: it cannot be invoked with a request to `/rest/`.

{% endnote %}

## How to Call the Command

The command is invoked from the widget with the [BX24.placement.call](../bx24-placement-call.md) method. The third argument is the callback function, which receives the result of the command.

```js
BX24.placement.call('disableAutoClose', {}, function (result) {
    console.log(result);
});
```

## Command Parameters

The command takes no parameters. Pass an empty object `{}` as the second argument.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            BX24.placement.call('disableAutoClose', {}, function (result) {
                console.log(result);
            });
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    await $b24.placement.call('disableAutoClose')
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        const result = await $b24.placement.call('disableAutoClose')

        console.log(result)
      })
    </script>
    ```

{% endlist %}

## Command Result

```json
[]
```

### Returned Data

An empty array on a successful call.

## Errors

The `disableAutoClose` command has no error codes of its own: it either runs or is not invoked at all.

- If the widget is open outside the `CALL_CARD` placement, the placement interface ignores the unknown command and the callback function is not invoked
- Check the command name with the correct capitalization: the list of commands available in the current placement is returned by [BX24.placement.getInterface](../bx24-placement-get-interface.md)

## Continue Learning

- [{#T}](./get-status.md)
- [{#T}](./enable-auto-close.md)
- [{#T}](./call-card-entity-changed.md)
- [{#T}](./call-card-before-close.md)
- [{#T}](./call-card-call-state-changed.md)
- [{#T}](./index.md)
- [{#T}](../../telephony/call-card.md)
