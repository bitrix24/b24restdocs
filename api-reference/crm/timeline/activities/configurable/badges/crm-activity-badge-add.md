# Add Badge crm.activity.badge.add

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../../scopes/permissions.md)
>
> Who can execute the method: a user with administrative access to the CRM section

The method `crm.activity.badge.add` registers a badge — an icon that appears on the card of a CRM object in the kanban.

The method only registers the badge and does not attach it to a CRM object. For the icon to appear, pass the badge code in the `badgeCode` field of a [configurable activity](../index.md) when calling [crm.activity.configurable.add](../crm-activity-configurable-add.md) or [crm.activity.configurable.update](../crm-activity-configurable-update.md).

## Method Parameters

{% include [Note on required parameters](../../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../../../../data-types.md) | Badge code, for example `missedCall`. No longer than 30 characters and unique ||
|| **title***
[`string`\|`object`](../../../../../data-types.md) | Badge name. A string, or an object with translations where the key is a language code ||
|| **value***
[`string`\|`object`](../../../../../data-types.md) | Text inside the icon, displayed in uppercase. A string or an object with translations ||
|| **type***
[`string`](../../../../../data-types.md) | [Badge type](./index.md#badge-type). Defines the icon color: `success`, `failure`, `warning`, `primary`, or `secondary` ||
|#

The codes already in use are returned by the [crm.activity.badge.list](./crm-activity-badge-list.md) method.

In an object with translations, the keys can only be language codes that Bitrix24 recognizes, for example `ru`, `en`, `de`.

## Code Examples

{% include [Note on examples](../../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"missedCall","title":"Call Status","value":"Missed","type":"failure"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.activity.badge.add
    ```

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
        // title and value come as a string or as an object with translations
        title: string | Record<string, string>
        value: string | Record<string, string>
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

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.activity.badge.add(
            code="missedCall",
            title="Call Status",
            value="Missed",
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

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.activity.badge.add", b24.Params{
    	"code":  "missedCall",
    	"title": "Call Status",
    	"value": "Missed",
    	"type":  "failure",
    })
    if err != nil {
    	return fmt.Errorf("crm.activity.badge.add: %w", err)
    }

    // The method wraps the response in an object with the "badge" key.
    raw, ok := b24.Unwrap(res.Result, "badge")
    if !ok {
    	return fmt.Errorf("no badge key in the response")
    }

    var item struct {
    	Code string `json:"code"`
    	// Title and Value come as a string or as an object with translations
    	Title any    `json:"title"`
    	Value any    `json:"value"`
    	Type  string `json:"type"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Code, item.Title)
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
[`object`](../../../../../data-types.md) | Root element of the response with a single **badge** key [(detailed description)](#badge) ||
|| **time**
[`time`](../../../../../data-types.md#time) | Information about the request execution time ||
|#

#### Badge Object {#badge}

#|
|| **Name**
`type` | **Description** ||
|| **code**
[`string`](../../../../../data-types.md) | Badge code. Pass it in the `badgeCode` field of a configurable activity ||
|| **title**
[`string`\|`object`](../../../../../data-types.md) | Badge name exactly as it was passed ||
|| **value**
[`string`\|`object`](../../../../../data-types.md) | Text inside the icon ||
|| **type**
[`string`](../../../../../data-types.md) | [Badge type](./index.md#badge-type) ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "BX_INVALID_VALUE",
    "error_description": "Code must be unique"
}
```

{% include notitle [error handling](../../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ACCESS_DENIED` | Insufficient permissions: the method is available only to a user with administrative access to the CRM section ||
|| `100` | A required parameter is missing. Its name comes in the error description, for example `Could not find value for parameter {title}` ||
|| `REQUIRED_ARG_MISSING` | The `title` or `value` field is empty, or was passed as neither a string nor an object ||
|| `WRONG_TYPE_VALUE` | The value of the `type` field is not in the list of allowed values ||
|| `BX_INVALID_VALUE` | A badge with this code is already registered: `Code must be unique` ||
|| `0` | General validation error code. The reason comes in `error_description`: `The length of the code field must not exceed 30 characters` — the code is longer than 30 characters, `` Language `X` was not found `` — an unknown language code is used in the object with translations ||
|#

{% include [system errors](../../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-activity-badge-get.md)
- [{#T}](./crm-activity-badge-list.md)
- [{#T}](./crm-activity-badge-delete.md)
- [{#T}](./index.md)
- [{#T}](../crm-activity-configurable-add.md)
