# Get the list of badges crm.activity.badge.list

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.activity.badge.list` returns the list of badges registered in Bitrix24. Each element of the list contains the [badge fields](./index.md#badge-fields).

The method returns every badge in a single response: it has no pagination and does not accept the `start` parameter. The total number of badges comes in the `total` field.

The list shows which codes are already in use: check it before calling [crm.activity.badge.add](./crm-activity-badge-add.md).

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.badge.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.activity.badge.list
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type BadgeListResult = {
      badges: {
        code: string
        // title and value come as a string or as an object with translations
        title: string | Record<string, string>
        value: string | Record<string, string>
        type: string
      }[]
    }

    try {
      // crm.activity.badge.list takes no parameters and returns every badge in one response
      const response = await $b24.actions.v2.call.make<BadgeListResult>({
        method: 'crm.activity.badge.list',
        params: {},
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Badges:', result.badges, 'Count:', result.badges.length)
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
      async function loadBadgeList() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          // crm.activity.badge.list takes no parameters and returns every badge in one response
          const response = await $b24.actions.v2.call.make({
            method: 'crm.activity.badge.list',
            params: {},
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Badges:', result.badges, 'Count:', result.badges.length)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', loadBadgeList)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.badge.list().response
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

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.activity.badge.list',
                []
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
        echo 'Error fetching activity badges: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.activity.badge.list",
        {
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
        'crm.activity.badge.list',
        []
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.badge.list", nil, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.activity.badge.list: %w", err)
    }

    // The method wraps the response in an object with the "badges" key.
    raw, ok := b24.Unwrap(res.Result, "badges")
    if !ok {
    	return fmt.Errorf("no badges key in the response")
    }

    var items []struct {
    	Code string `json:"code"`
    	// Title and Value come as a string or as an object with translations
    	Title any    `json:"title"`
    	Value any    `json:"value"`
    	Type  string `json:"type"`
    }
    if err := json.Unmarshal(raw, &items); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    for _, it := range items {
    	fmt.Println(it.Code)
    }
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "badges": [
            {
                "code": "missedCall",
                "title": "Call Status",
                "value": "Missed",
                "type": "failure"
            }
        ]
    },
    "total": 1,
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
[`object`](../../../../../data-types.md) | Root element of the response with a single **badges** key [(detailed description)](#badges) ||
|| **total**
[`integer`](../../../../../data-types.md) | Number of badges in the response ||
|| **time**
[`time`](../../../../../data-types.md#time) | Information about the execution time of the request ||
|#

#### Badges Array {#badges}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../../../../data-types.md) | Badge code. Use it to set the badge in the `badgeCode` field of an activity, to retrieve the badge with [crm.activity.badge.get](./crm-activity-badge-get.md), and to delete it with [crm.activity.badge.delete](./crm-activity-badge-delete.md) ||
|| **title**
[`string`\|`object`](../../../../../data-types.md) | Badge name. A string, or an object with translations if the badge was added in several languages ||
|| **value**
[`string`\|`object`](../../../../../data-types.md) | Text inside the icon. A string or an object with translations ||
|| **type**
[`string`](../../../../../data-types.md) | [Badge type](./index.md#badge-type): `success`, `failure`, `warning`, `primary`, or `secondary` ||
|#

If there are no badges, the method returns an empty array and `total` with the value `0`. The order of elements in the array is not guaranteed — if you need a specific order, sort the list on your side.

## Error Handling

{% include notitle [error handling](../../../../../../_includes/error-info.md) %}

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-add.md)
- [{#T}](./crm-activity-badge-get.md)
- [{#T}](./crm-activity-badge-delete.md)
- [{#T}](./index.md)
- [{#T}](../crm-activity-configurable-add.md)

