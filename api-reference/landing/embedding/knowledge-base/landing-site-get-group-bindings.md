# Get Group Bindings - landing.site.getGroupBindings

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with View access permission in the Sites section

The method `landing.site.getGroupBindings` returns the bindings of Knowledge Bases to groups.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **groupId**
[`integer`](../../../data-types.md) \| [`null`](../../../data-types.md) | The identifier of the group for filtering.

If not provided, bindings for all groups will be returned.

`groupId` can be obtained from the group interface or from the result of the current method in the `BINDING_ID` field for existing bindings ||
|#

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Example of retrieving group bindings, where:
- `groupId` — the identifier of the group for filtering

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "groupId": 174
      }' \
      "https://**put.your-domain-here**/rest/**user_id**/**webhook_code**/landing.site.getGroupBindings.json"
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -d '{
        "groupId": 174,
        "auth": "**put_access_token_here**"
      }' \
      "https://**put.your-domain-here**/rest/landing.site.getGroupBindings.json"
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of each GroupBindingItem returned in result[]
    type GroupBindingItem = {
      ENTITY_ID: string
      ENTITY_TYPE: string
      BINDING_ID: string
      TITLE: string
      PUBLIC_URL: string
    }

    try {
      const response = await $b24.actions.v2.call.make<GroupBindingItem[]>({
        method: 'landing.site.getGroupBindings',
        params: {
          groupId: 174,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Group bindings count:', result.length, result)
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
      async function getGroupBindings() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'landing.site.getGroupBindings',
            params: {
              groupId: 174,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Group bindings count:', result.length, result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getGroupBindings)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'landing.site.getGroupBindings',
                [
                    'groupId' => 174,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error getting group bindings: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'landing.site.getGroupBindings',
        {
            groupId: 174
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
        'landing.site.getGroupBindings',
        [
            'groupId' => 174,
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
            "ENTITY_ID": "65",
            "ENTITY_TYPE": "S",
            "BINDING_ID": "5",
            "TITLE": "Knowledge Base in Dark Theme",
            "PUBLIC_URL": "https://bitrix24.com/knowledge/group/knowledge_base_in_dark_theme/"
        },
        {
            "ENTITY_ID": "41",
            "ENTITY_TYPE": "S",
            "BINDING_ID": "119",
            "TITLE": "Knowledge Base",
            "PUBLIC_URL": "https://bitrix24.com/knowledge/group/knowledge_base/"
        }
    ],
    "time": {
        "start": 1774956574,
        "finish": 1774956574.718824,
        "duration": 0.7188239097595215,
        "processing": 0,
        "date_start": "2026-03-31T14:29:34+01:00",
        "date_finish": "2026-03-31T14:29:34+01:00",
        "operating_reset_at": 1774957174,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object[]`](../../../data-types.md) | List of group bindings [more details](#group-binding-item) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

### Result Item Type {#group-binding-item}

#|
|| **Name**
`type` | **Description** ||
|| **ENTITY_ID**
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Identifier of the site ||
|| **ENTITY_TYPE**
[`string`](../../../data-types.md) | Type of the object:

- `S` — site ||
|| **BINDING_ID**
[`integer`](../../../data-types.md) \| [`string`](../../../data-types.md) | Identifier of the group ||
|| **TITLE**
[`string`](../../../data-types.md) | Title of the bound site ||
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
|| `TYPE_ERROR` | Data type error | The `groupId` parameter is passed in an incompatible type ||
|| `ACCESS_DENIED` | Insufficient permissions | The user did not pass general access checks ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./landing-site-binding-to-group.md)
- [{#T}](./landing-site-unbinding-from-group.md)
- [{#T}](./landing-site-get-menu-bindings.md)
- [{#T}](./landing-site-binding-to-menu.md)
- [{#T}](./index.md)