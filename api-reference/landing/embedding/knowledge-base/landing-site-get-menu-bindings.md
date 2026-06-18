# Get Menu Bindings with landing.site.getMenuBindings

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section

The method `landing.site.getMenuBindings` returns Knowledge Base bindings to the menu.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **menuCode**
[`string`](../../../data-types.md) \| [`null`](../../../data-types.md) | Menu code for filtering.

If not provided, bindings for all menus will be returned.

`menuCode` can be obtained:
- in the interface through the "Select Knowledge Base" option: in the URL of the opened frame, the `menuId` parameter contains the menu code (for example, `menuId=crm_switcher:deal`)
- from the result of the method [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) in the `BINDING_ID` field ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

Example of obtaining menu bindings, where:
- `menuCode` — menu code for filtering

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "menuCode": "crm_switcher:deal"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getMenuBindings.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "menuCode": "crm_switcher:deal",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getMenuBindings.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each MenuBinding returned in result[]
    type MenuBinding = {
      ENTITY_ID: string | number
      ENTITY_TYPE: string
      BINDING_ID: string
      TITLE: string
      PUBLIC_URL: string
    }

    try {
      const response = await $b24.actions.v2.call.make<MenuBinding[]>({
        method: 'landing.site.getMenuBindings',
        params: {
          menuCode: 'crm_switcher:deal',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Menu bindings count:', result.length, 'First binding:', result[0])
      }
    } catch (error) {
      // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <!-- Load the SDK (UMD build); it is exposed as the global B24Js -->
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function getMenuBindings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.getMenuBindings',
            params: {
              menuCode: 'crm_switcher:deal',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Menu bindings count:', result.length, 'First binding:', result[0])
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getMenuBindings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.site.getMenuBindings',
                [
                    'menuCode' => 'crm_switcher:deal',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting menu bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getMenuBindings',
        {
            menuCode: 'crm_switcher:deal'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error());
            }
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'landing.site.getMenuBindings',
        [
            'menuCode' => 'crm_switcher:deal',
        ]
    );

    if (isset($result['error']))
    {
        echo 'Error: ' . $result['error_description'];
    }
    else
    {
        echo '<pre>';
        print_r($result['result']);
        echo '</pre>';
    }
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": [
        {
            "ENTITY_ID": "39",
            "ENTITY_TYPE": "S",
            "BINDING_ID": "socialnetwork:group_notifications",
            "TITLE": "Knowledge Base",
            "PUBLIC_URL": "https://bitrix24.com/knowledge/knowledge_base/"
        }
    ],
    "time": {
        "start": 1774956995,
        "finish": 1774956995.125436,
        "duration": 0.12543606758117676,
        "processing": 0,
        "date_start": "2026-03-31T14:36:35+02:00",
        "date_finish": "2026-03-31T14:36:35+02:00",
        "operating_reset_at": 1774957595,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | List of menu bindings [more details](#menu-binding-item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Result Item Type {#menu-binding-item}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Site identifier ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Object type:

- `S` — site
- `L` — landing ||
|| **BINDING_ID**
[`string`](../../../data-types.md) | Menu code ||
|| **TITLE**
[`string`](../../../data-types.md) | Name of the bound site ||
|| **PUBLIC_URL**
[`string`](../../../data-types.md) | Public URL of the bound site ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Insufficient permissions."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `TYPE_ERROR` | Data type error | The `menuCode` parameter is passed in an incompatible type ||
|| `ACCESS_DENIED` | Insufficient permissions | The user did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-binding-to-menu.md)
- [{#T}](./landing-site-unbinding-from-menu.md)
- [{#T}](./landing-site-get-group-bindings.md)
- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./index.md)