# Update External Line telephony.externalLine.update

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalLine.update` modifies the parameters of an external line.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NUMBER***
[`string`](../data-types.md) | The number of the external line.

The number can be obtained using the [telephony.externalLine.get](./telephony-external-line-get.md) method. Leading and trailing spaces are removed.

An application modifies only its own lines, and a webhook only the lines added via an inbound webhook ||
|| **NAME**
[`string`](../data-types.md) | The new name of the external line.

If the parameter is not provided, the name does not change ||
|| **CRM_AUTO_CREATE**
[`string`](../data-types.md) | Auto-creation of a CRM object when an employee calls a number from a CRM card through this line. The flag does not affect the [telephony.externalCall.register](./telephony-external-call-register.md) method, where a CRM object is created based on the `CRM_CREATE` parameter.

Possible values:
- `Y` — enabled
- `N` — disabled

Any value other than `Y` is saved as `N`. If the parameter is not provided, the value does not change ||
|#

{% note info "" %}

At least one field must be provided for modification: `NAME` or `CRM_AUTO_CREATE`. Otherwise, you will receive the `There are no fields to update` error

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NUMBER":"14151234567","NAME":"Support Line","CRM_AUTO_CREATE":"N"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/telephony.externalLine.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NUMBER":"14151234567","NAME":"Support Line","CRM_AUTO_CREATE":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalLine.update
    ```

- JS (TS)

    ```ts
    // This snippet is an ES module: top-level await requires type="module" or a bundler.
    // $b24 is an already-initialized SDK instance (see the SDK "Get started" guide).
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    // Shape of the payload returned in result (match the "response handling" section of the page)
    type ExternalLineUpdateResult = {
      ID: string
    }

    try {
      const response = await $b24.actions.v2.call.make<ExternalLineUpdateResult>({
        method: 'telephony.externalLine.update',
        params: {
          NUMBER: '14151234567',
          NAME: 'Support line',
          CRM_AUTO_CREATE: 'N',
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Updated external line ID:', result.ID)
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
      async function updateExternalLine() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'telephony.externalLine.update',
            params: {
              NUMBER: '14151234567',
              NAME: 'Support line',
              CRM_AUTO_CREATE: 'N',
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Updated external line ID:', result.ID)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', updateExternalLine)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.telephony.external_line.update(
            number="14151234567",
            name="Support line",
            crm_auto_create=False,
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
                'telephony.externalLine.update',
                [
                    'NUMBER' => '14151234567',
                    'NAME' => 'Support Line',
                    'CRM_AUTO_CREATE' => 'N'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating external line: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalLine.update",
        {
            NUMBER: '14151234567',
            NAME: 'Support Line',
            CRM_AUTO_CREATE: 'N'
        },
        function(result)
        {
            if (result.error())
            {
                console.error(result.error(), result.error_description());
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'telephony.externalLine.update',
        [
            'NUMBER' => '14151234567',
            'NAME' => 'Support Line',
            'CRM_AUTO_CREATE' => 'N'
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "telephony.externalLine.update", b24.Params{
    	"NUMBER":          "14151234567",
    	"NAME":            "Support Line",
    	"CRM_AUTO_CREATE": "N",
    })
    if err != nil {
    	return fmt.Errorf("telephony.externalLine.update: %w", err)
    }

    var item struct {
    	ID b24.ID `json:"ID"`
    }
    if err := json.Unmarshal(res.Result, &item); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println(item.ID)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": {
        "ID": "7"
    },
    "time": {
        "start": 1772806119,
        "finish": 1772806120.006578,
        "duration": 1.006577968597412,
        "processing": 1,
        "date_start": "2026-03-06T17:08:39+02:00",
        "date_finish": "2026-03-06T17:08:40+02:00",
        "operating_reset_at": 1772806719,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response [(detailed description)](#result) ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

#### Object result {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`string`](../data-types.md) | The identifier of the updated external line. The number is returned as a string ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | The method was called neither with an application OAuth token nor via an inbound webhook, for example, from a browser session ||
|| `400` | Empty value | There are no fields to update | No fields provided for modification `NAME` or `CRM_AUTO_CREATE` ||
|| `400` | Empty value | NUMBER should not be empty | Required parameter `NUMBER` not provided ||
|| `400` | Empty value | Could not find line with number {NUMBER} | Line with the specified number not found ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-line-add.md)
- [{#T}](./telephony-external-line-get.md)
- [{#T}](./telephony-external-line-delete.md)
- [{#T}](./telephony-external-call-register.md)