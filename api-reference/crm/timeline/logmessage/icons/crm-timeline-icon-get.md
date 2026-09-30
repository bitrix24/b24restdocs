# Get Information About The crm.timeline.icon.get

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

The `crm.timeline.icon.get` method retrieves information about a timeline log entry icon.

## Method Parameters

{% include [Note on required parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **code***
[`string`](../../../../data-types.md) | Icon code (for example, `custom-info`).

You can get a list of all available codes using the method [`crm.timeline.icon.list`](./crm-timeline-icon-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"custom-info"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.icon.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"code":"custom-info","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.icon.get
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type IconGetResult = {
      icon: {
        code: string
        isSystem: boolean
        fileUri: string
      }
    }

    try {
      const response = await $b24.actions.v2.call.make<IconGetResult>({
        method: 'crm.timeline.icon.get',
        params: {
          code: 'custom-info',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info(result.icon.code, result.icon.isSystem, result.icon.fileUri)
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
      async function getTimelineIcon() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'crm.timeline.icon.get',
            params: {
              code: 'custom-info',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info(result.icon.code, result.icon.isSystem, result.icon.fileUri)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', getTimelineIcon)
    </script>
    ```

- Python

    ```python

    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.crm.timeline.icon.get(
            code="custom-info",
        )
        result = bitrix_response.response.result
        print(result)
    except BitrixAPIError as error:
        print(
            "Bitrix API Error",
            f"error: {error.error}",
            f"error_description: {error.error_description}",
            sep="\n",
        )
    except BitrixSDKException as error:
        print(f"Bitrix SDK Error: {error.message}")
    except Exception as error:
        print(f"Unexpected error: {error}")
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'crm.timeline.icon.get',
                [
                    'code' => 'custom-info',
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
        echo 'Error getting timeline icon: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.timeline.icon.get",
        {
            code: "custom-info",
        },
        result => {
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
        'crm.timeline.icon.get',
        [
            'code' => 'custom-info'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "crm.timeline.icon.get", b24.Params{
        "code": "custom-info",
    }, b24.WithIdempotent())
    if err != nil {
    	return fmt.Errorf("crm.timeline.icon.get: %w", err)
    }

    // The method wraps the response in an object with the "icon" key.
    raw, ok := b24.Unwrap(res.Result, "icon")
    if !ok {
    	return fmt.Errorf("no icon key in the response")
    }

    var item struct {
    	Code     string `json:"code"`
    	IsSystem bool   `json:"isSystem"`
    	FileUri  string `json:"fileUri"`
    }
    if err := json.Unmarshal(raw, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.Code, item.IsSystem)
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": {
        "icon": {
            "code": "custom-info",
            "isSystem": false,
            "fileUri": "/upload/crm/13f/huhnvzds7ckoy6mk5mdze9pb7jqscpxi/e66fm2cbau9f8u32oe9jzx2qflqhj2vv"
        }
    },
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response.

The `result` field contains an [object with icon data](#result) ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

#### result Object {#result}

#|
|| **Field**
`type` | **Description** ||
|| **icon**
[`object`](../../../../data-types.md) | [Icon](#icon) data ||
|#

#### icon Object {#icon}

#|
|| **Field**
`type` | **Description** ||
|| **code**
[`string`](../../../../data-types.md) | Icon code ||
|| **isSystem**
[`boolean`](../../../../data-types.md) | System icon indicator:

- `true` — system icon
- `false` — custom icon

||
|| **fileUri**
[`string`](../../../../data-types.md) | Path to the custom icon file. For a system icon, an empty string is returned ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "NOT_FOUND",
    "error_description": "Icon not found for code `test`"
}
```

{% include notitle [Error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `NOT_FOUND` | Icon not found for code `{code}` | An icon with the specified `code` does not exist ||
|| `400` | `100` | Could not find value for parameter {code} | The required `code` parameter was not provided ||
|#

{% include [System errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-timeline-icon-add.md)
- [{#T}](./crm-timeline-icon-list.md)
- [{#T}](./crm-timeline-icon-delete.md)
