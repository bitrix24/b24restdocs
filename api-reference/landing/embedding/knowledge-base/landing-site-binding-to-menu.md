# Bind Knowledge Base to Menu landing.site.bindingToMenu

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with View permission in the Sites section and Placement permission in the Extensions section of the Knowledge Base

The method `landing.site.bindingToMenu` binds the Knowledge Base to the specified menu.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id**^*^
[`integer`](../../../data-types.md) | Identifier of the Knowledge Base site.

`id` can be obtained, for example, from the method [landing.site.getList](../../site/landing-site-get-list.md) in the `ID` field ||
|| **menuCode**^*^
[`string`](../../../data-types.md) | Menu code.

`menuCode` can be obtained:
- in the interface through the "Select Knowledge Base" option: in the URL of the opened frame, the `menuId` parameter contains the menu code (for example, `menuId=crm_switcher:deal`)
- from the result of the method [landing.site.getMenuBindings](./landing-site-get-menu-bindings.md) in the `BINDING_ID` field ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Example of binding the Knowledge Base to a menu, where:
- `id` — identifier of the Knowledge Base site
- `menuCode` — menu code

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 31,
        "menuCode": "crm_switcher:deal"
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.bindingToMenu.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "id": 31,
        "menuCode": "crm_switcher:deal",
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.bindingToMenu.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<boolean>({
        method: 'landing.site.bindingToMenu',
        params: {
          id: 31,
          menuCode: 'crm_switcher:deal',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Binding result:', result)
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
      async function bindSiteToMenu() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.bindingToMenu',
            params: {
              id: 31,
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
          console.info('Binding result:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', bindSiteToMenu)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.site.bindingToMenu',
                [
                    'id' => 31,
                    'menuCode' => 'crm_switcher:deal',
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . var_export($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error binding site to menu: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.bindingToMenu',
        {
            id: 31,
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
        'landing.site.bindingToMenu',
        [
            'id' => 31,
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
    "result": true,
    "time": {
        "start": 1774952664,
        "finish": 1774952665.017161,
        "duration": 1.0171608924865723,
        "processing": 0,
        "date_start": "2026-03-31T13:24:24+03:00",
        "date_finish": "2026-03-31T13:24:25+03:00",
        "operating_reset_at": 1774953265,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Binding result:

- `true` — binding completed
- `false` — binding not completed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "MISSING_PARAMS",
    "error_description": "Not enough call parameters, missing: menuCode"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `MISSING_PARAMS` | Not enough call parameters, missing: menuCode | Method call without `menuCode` ||
|| `MISSING_PARAMS` | Not enough call parameters, missing: id | Method call without `id` ||
|| `TYPE_ERROR` | Bitrix\\Landing\\PublicAction\\Site::bindingToMenu(): Argument #1 ($id) must be of type int, string given | Values provided are incompatible with the method signature ||
|| `ACCESS_DENIED` | Insufficient permissions | User did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-unbinding-from-menu.md)
- [{#T}](./landing-site-get-menu-bindings.md)
- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./landing-site-get-group-bindings.md)
- [{#T}](./index.md)