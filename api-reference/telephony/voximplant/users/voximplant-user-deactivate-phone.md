# Deactivate User SIP Device voximplant.user.deactivatePhone

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not to be deployed to prod_" %}

Do not publish this page until the method is fixed and retested. On a test Bitrix24 account, the method activates the SIP device instead of deactivating it.

{% endnote %}

{% endif %}

> Scope: [`telephony`](../../../scopes/permissions.md)
>
> Who can execute the method: a user with the User Settings — Modify permission

The method `voximplant.user.deactivatePhone` deactivates a user's SIP device.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **USER_ID***
[`integer`](../../../data-types.md) | Identifier of the user whose SIP device should be deactivated.

You can retrieve the identifier using [user.get](../../../user/user-get.md)
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":1269}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.user.deactivatePhone
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"USER_ID":1269,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.user.deactivatePhone
    ```

- JS (TS)

    ```ts
    import { Text } from '@bitrix24/b24jssdk'
    import type { B24Frame } from '@bitrix24/b24jssdk'

    declare const $b24: B24Frame

    try {
      const response = await $b24.actions.v2.call.make<number>({
        method: 'voximplant.user.deactivatePhone',
        params: {
          USER_ID: 1269,
        },
        requestId: Text.getUuidRfc4122()
      })

      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Phone deactivated, result:', result)
      }
    } catch (error) {
      console.error(error)
    }
    ```

- JS (UMD)

    ```html
    <script src="https://unpkg.com/@bitrix24/b24jssdk@1/dist/umd/index.min.js"></script>
    <script>
      async function deactivatePhone() {
        try {
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'voximplant.user.deactivatePhone',
            params: {
              USER_ID: 1269,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Phone deactivated, result:', result)
        } catch (error) {
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deactivatePhone)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.voximplant.user.deactivate_phone(
            user_id=1269,
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
                'voximplant.user.deactivatePhone',
                [
                    'USER_ID' => 1269,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.user.deactivatePhone',
        {
            USER_ID: 1269
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
        'voximplant.user.deactivatePhone',
        [
            'USER_ID' => 1269,
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "voximplant.user.deactivatePhone", b24.Params{
        "USER_ID": 1269,
    })
    if err != nil {
        return fmt.Errorf("voximplant.user.deactivatePhone: %w", err)
    }

    var value b24.ID
    if err := json.Unmarshal(res.Result, &value); err != nil {
        return fmt.Errorf("parse response: %w", err)
    }
    fmt.Println("result:", value)
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": 1,
    "time": {
        "start": 1773666334,
        "finish": 1773666335.198412,
        "duration": 1.1984119415283203,
        "processing": 1,
        "date_start": "2026-03-16T16:05:34+03:00",
        "date_finish": "2026-03-16T16:05:35+03:00",
        "operating_reset_at": 1773666934,
        "operating": 0.4791221618652344
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../data-types.md) | Method execution result.

`1` — the SIP device deactivation request was completed ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "",
    "error_description": "Parameter USER_ID is not set"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| Empty value | `Parameter USER_ID is not set` | Required parameter `USER_ID` is not specified ||
|| Empty value | `You are not allowed to modify user's settings` | Insufficient permissions to deactivate the user's SIP device ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./index.md)
- [{#T}](./voximplant-user-get.md)
- [{#T}](./voximplant-user-activate-phone.md)
