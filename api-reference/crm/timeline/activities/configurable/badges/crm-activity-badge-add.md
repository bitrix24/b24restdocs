# Add Badge crm.activity.badge.add

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: users with administrative access to the crm section

The method `crm.activity.badge.add` adds a new badge for a configurable activity.

## Method Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Field** | **Description** ||
|| **code***
[`string`](../../../../../data-types.md) | Badge code, for example `missedCall` ||
|| **title***
[`string`\|`array`](../../../../../data-types.md) | Badge title. Can be a string or an array of strings for different languages ||
|| **value***
[`string`\|`array`](../../../../../data-types.md) | Badge value. Can be a string or an array of strings for different languages ||
|| **type***
[`string`](../../../../../data-types.md) | [Badge type](./index.md#badge-type) ||
|#

## Code Examples

{% include [Note on examples](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"missedCall","title":"Call Status","value":"Missed","type":"failure","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.badge.add
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BadgeAddResult = {
      badge: {
        code: string
        title: string
        value: string
        type: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<BadgeAddResult>({
        method: 'crm.activity.badge.add',
        params: {
          code: 'missedCall',
          title: 'Call Status',
          value: 'Missed',
          type: 'failure',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.badge)
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
      async function addActivityBadge() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.badge.add',
            params: {
              code: 'missedCall',
              title: 'Call Status',
              value: 'Missed',
              type: 'failure',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.badge)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', addActivityBadge)
    </script>
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.badge.add',
                [
                    'code'  => 'missedCall',
                    'title' => 'Call Status',
                    'value' => 'Missed',
                    'type'  => 'failure'
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding activity badge: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.badge.add",
        {
            code: 'missedCall',
            title: 'Call Status',
            value: 'Missed',
            type: 'failure'
        }, result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }    
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.activity.badge.add',
        [
            'code' => 'missedCall',
            'title' => 'Call Status',
            'value' => 'Missed',
            'type' => 'failure'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Python

    Example

    ```python
    from b24pysdk.client import BaseClient
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    client: BaseClient

    try:
        bitrix_response = client.crm.activity.badge.add(
            code="CUSTOM_STATUS",
            title="Status",
            value="Pending",
            type="failure",
        ).response
        result = bitrix_response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```
{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "badge": {
            "code": "missedCall",
            "title": "Call Status",
            "value": "Missed",
            "type": "failure"
        }
    },
    "time": {
        "start": 1724068028.331234,
        "finish": 1724068028.726591,
        "duration": 0.3953571319580078,
        "processing": 0.13033390045166016,
        "date_start": "2025-01-21T13:47:08+02:00",
        "date_finish": "2025-01-21T13:47:08+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response containing information about the added badge in case of success. In case of failure, it will return `null` ||
|| **time**
[`time`](../../../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Not found."
}
```

{% include notitle [error handling](../../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions to perform the operation ||
|| `REQUIRED_ARG_MISSING` | Required fields are not filled ||
|#

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-get.md)
- [{#T}](./crm-activity-badge-list.md)
- [{#T}](./crm-activity-badge-delete.md)