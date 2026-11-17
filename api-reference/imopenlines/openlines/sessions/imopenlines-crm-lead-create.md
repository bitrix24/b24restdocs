# Create a Lead Based on the Dialogue imopenlines.crm.lead.create

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: the operator handling the dialogue

The method `imopenlines.crm.lead.create` creates a CRM lead from the client data in an Open Channels dialogue.

If the `CRM_CREATE` parameter in the channel settings is `deal`, the method creates a deal with the client's contact in the pipeline from `CRM_CREATE_SECOND` instead of a lead. You can retrieve the channel settings using the [imopenlines.config.get](../imopenlines-config-get.md) method.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Numeric identifier of the Open Channels chat without the `chat` prefix.

The identifier can be obtained using the [imopenlines.session.open](./imopenlines-session-open.md) or [imopenlines.dialog.get](./imopenlines-dialog-get.md) methods. ||
|#

If the dialogue is already linked to CRM, the method does not create a new object and returns `true`. The CRM link is shown by the first element of the `entity_data_1` string in the response of the [imopenlines.dialog.get](./imopenlines-dialog-get.md) method: the value `Y` means the link exists.

After a lead or deal is created, a system message with a link to the created object is posted to the dialogue chat.

## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.crm.lead.create
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"CHAT_ID":2043,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/imopenlines.crm.lead.create
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
        method: 'imopenlines.crm.lead.create',
        params: {
          CHAT_ID: 2043,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Lead created:', result)
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
      async function createCrmLead() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'imopenlines.crm.lead.create',
            params: {
              CHAT_ID: 2043,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Lead created:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', createCrmLead)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.imopenlines.crm.lead.create(
            chat_id=2043,
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
                'imopenlines.crm.lead.create',
                [
                    'CHAT_ID' => 2043,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        print_r($result);
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.crm.lead.create',
        {
            CHAT_ID: 2043,
        },
        function(result) {
            if (result.error()) {
                console.error(result.error().ex);
            } else {
                console.log(result.data());
            }
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'imopenlines.crm.lead.create',
        [
            'CHAT_ID' => 2043,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Success: ' . print_r($result['result'], true);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "imopenlines.crm.lead.create", b24.Params{
    	"CHAT_ID": 2043,
    })
    if err != nil {
    	return fmt.Errorf("imopenlines.crm.lead.create: %w", err)
    }

    var ok bool
    if err := json.Unmarshal(res.Result, &ok); err != nil {
    	return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("done:", ok)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773683282,
        "finish": 1773683283.095389,
        "duration": 1.0953888893127441,
        "processing": 1,
        "date_start": "2026-03-16T20:48:02+01:00",
        "date_finish": "2026-03-16T20:48:03+01:00",
        "operating_reset_at": 1773683882,
        "operating": 0.8203368186950684
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Returns `true` if a lead or deal is created or the dialogue is already linked to CRM ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CHAT_TYPE",
    "error_description": "The specified chat is not an Open Channel"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `400` | `CHAT_ID` | Chat ID is invalid. | `CHAT_ID` is not passed, is less than `1`, or the chat is not found ||
|| `400` | `CHAT_TYPE` | This chat is not Open Channel | The chat does not belong to an Open Channel ||
|| `400` | `ACCESS_DENIED` | You do not have permission to access this chat. | The user is not a member of the chat and does not have the "Join conversation" permission for the channel ||
|| `400` | `USER_ID` | User ID is invalid. | User not defined for whom the method is executed ||
|| `400` | `ERROR_USER_NOT_OPERATOR` | Attempt to save a CRM entity by a user who is not an operator | The method was called by a user who is not handling this dialogue ||
|| `400` | `IMOL_CHAT_ERROR_ANSWER_NOT_LOAD_SESSION` | Cannot load session | The chat has no Open Channels session ||
|| `400` | `IMOL_CHAT_ERROR_NOT_LOAD_CRM` | Failed to load CRM | The CRM module is unavailable ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-session-open.md)
- [{#T}](./imopenlines-session-start.md)
- [{#T}](./imopenlines-session-join.md)
- [{#T}](./imopenlines-session-history-get.md)
- [{#T}](./imopenlines-session-intercept.md)
- [{#T}](./imopenlines-session-mode-pin.md)
- [{#T}](./imopenlines-session-mode-pin-all.md)
- [{#T}](./imopenlines-session-mode-unpin-all.md)
- [{#T}](./imopenlines-session-mode-silent.md)
- [{#T}](./imopenlines-session-head-vote.md)
- [{#T}](./imopenlines-message-session-start.md)
- [{#T}](./imopenlines-dialog-get.md)