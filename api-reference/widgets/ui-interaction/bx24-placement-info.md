# Get the Widget Execution Context BX24.placement.info

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`placement`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `BX24.placement.info` retrieves the handler call context: the code of the placement where the widget is open and the parameters passed along with it.

```js
BX24.placement.info();
```

## Parameters

The method takes no parameters and returns the result immediately, without a callback function.

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- BX24.js

    ```js
    BX24.ready(function () {
        BX24.init(function () {
            const placementInfo = BX24.placement.info();

            console.info(placementInfo.placement, placementInfo.options);
        });
    });
    ```

- JS (TS)

    ```ts
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide)
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // the same data is available as properties of the placement manager
    console.info($b24.placement.placement, $b24.placement.options)
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      document.addEventListener('DOMContentLoaded', async () => {
        const $b24 = await B24Js.initializeB24Frame()

        console.info($b24.placement.placement, $b24.placement.options)
      })
    </script>
    ```

{% endlist %}

## Result

```json
{"placement":"CRM_LEAD_LIST_MENU","options":{"ID":"1348"}}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **placement**
[`string`](../../data-types.md) | The code of the placement where the widget is open. For a widget opened through the main application link — `DEFAULT` ||
|| **options**
[`object`](../../data-types.md) | The placement parameters. The set of keys depends on the placement: for example, the identifier of a CRM item or the identifier of a chat. The keys of each placement are described on its page in the [placement catalog](../placements.md) ||
|#

## Errors

The method returns no errors. If `placement` contains `DEFAULT` while a placement was expected, the widget was opened through the main application link rather than from a registered placement.

## Continue Learning

- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-call.md)
- [{#T}](bx24-placement-bind-event.md)
- [{#T}](../placements.md)
- [{#T}](index.md)
