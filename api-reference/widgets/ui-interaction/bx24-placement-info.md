# Get the Widget Execution Context BX24.placement.info

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

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

The code runs in the widget frame after the library is initialized: in [BX24.js](../../../sdk/bx24-js-sdk/index.md) — inside [BX24.init](../../../sdk/bx24-js-sdk/system-functions/bx24-init.md), in [b24jssdk](../../../sdk/b24jssdk/index.md) — after `initializeB24Frame`.

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
[`string`](../../data-types.md) | The code of the placement where the widget is open. `DEFAULT` — if the application is opened without a placement, for example through the main link ||
|| **options**
[`object`](../../data-types.md) | The data that Bitrix24 passes to the widget when it opens: for example, the identifier of a CRM item or the call data. The set of keys depends on the placement and is described on its page in the [placement catalog](../placements.md).

Numbers and boolean values may arrive as strings: for example, `"CRM_ENTITY_ID": "0"` and `"CALL_LIST_MODE": "false"` in the call card. Convert them to the required type yourself.

The `OPTIONS` parameters that the application passed to [placement.bind](../placement-bind.md) during registration are not included in `options` ||
|#

## Errors

The method returns no errors. If `placement` contains `DEFAULT` while a placement was expected, the application was opened without a placement: through the main link or with the [BX24.openApplication](../../../sdk/bx24-js-sdk/additional-functions/bx24-open-application.md) method. Check how the widget is opened.

## Continue Learning

- [{#T}](bx24-placement-get-interface.md)
- [{#T}](bx24-placement-call.md)
- [{#T}](bx24-placement-bind-event.md)
- [{#T}](../placements.md)
- [{#T}](index.md)
