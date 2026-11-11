# Delete Notification im.notify.delete

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.delete` removes a notification by `ID` or by tags `TAG` and `SUB_TAG`.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **ID**
[`integer`](../../data-types.md)
[`array`](../../data-types.md) | A notification identifier or an array of identifiers. The identifier is returned in the `result` field by the sending methods [im.notify](./im-notify.md), [im.notify.personal.add](./im-notify-personal-add.md), and [im.notify.system.add](./im-notify-system-add.md), and also by the [im.notify.get](./im-notify-get.md) method.

The method deletes only notifications whose recipient is the current user. If no such notification is found, the method returns `false` ||
|| **TAG**
[`string`](../../data-types.md) | The tag passed in `TAG` when the notification was sent. The method deletes notifications of the current application with this tag for all recipients.

Notifications sent via a webhook have no tags — delete them by `ID` ||
|| **SUB_TAG**
[`string`](../../data-types.md) | The tag passed in `SUB_TAG` when the notification was sent. The method deletes notifications of the current application with this tag for all recipients ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | The identifier the method uses to determine the application key when deleting by `TAG` or `SUB_TAG` via a webhook. Without it, a webhook call without `ID` returns the `ACCESS_DENIED` error.

Not required when calling from an application ||
|#

{% note warning "" %}

Pass one of the parameters: `ID`, `TAG`, or `SUB_TAG`. If several are passed, the method uses the first one in this order: `ID`, then `TAG`, then `SUB_TAG`.

{% endnote %}

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":101}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"ID":101,"auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.delete
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
        method: 'im.notify.delete',
        params: {
          ID: 101,
        },
        requestId: Text.getUuidRfc4122()
      })

      // The payload is available only on a successful response
      if (!response.isSuccess) {
        console.error(response.getErrorMessages().join('; '))
      } else {
        const result = response.getData()!.result
        console.info('Notification deleted:', result)
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
      async function deleteNotification() {
        try {
          // Initialize the SDK inside a Bitrix24 frame
          const $b24 = await B24Js.initializeB24Frame()

          const response = await $b24.actions.v2.call.make({
            method: 'im.notify.delete',
            params: {
              ID: 101,
            },
            requestId: B24Js.Text.getUuidRfc4122()
          })

          // The payload is available only on a successful response
          if (!response.isSuccess) {
            console.error(response.getErrorMessages().join('; '))
            return
          }

          const result = response.getData().result
          console.info('Notification deleted:', result)
        } catch (error) {
          // Thrown on transport or SDK failures (AjaxError, SdkError, etc.)
          console.error(error)
        }
      }

      document.addEventListener('DOMContentLoaded', deleteNotification)
    </script>
    ```

- Python

    ```python
    from b24pysdk.errors import BitrixAPIError, BitrixSDKException

    try:
        bitrix_response = client.im.notify.delete(
            bitrix_id=101,
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
        $response = $b24Service->core->call(
            'im.notify.delete',
            [
                'ID' => 101,
            ]
        );

        $result = $response->getResponseData()->getResult();

        print_r($result);
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.notify.delete',
        {
            ID: 101,
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
        'im.notify.delete',
        [
            'ID' => 101,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```

- Go

    ```go
    // client and ctx are already created — see the Go SDK section
    res, err := client.Core().Call(ctx, "im.notify.delete", b24.Params{
    	"ID": 101,
    })
    if err != nil {
    	return fmt.Errorf("im.notify.delete: %w", err)
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
        "start": 1794317795,
        "finish": 1794317795.872754,
        "duration": 0.8727540969848633,
        "processing": 0,
        "date_start": "2026-11-10T16:36:35+01:00",
        "date_finish": "2026-11-10T16:36:35+01:00",
        "operating_reset_at": 1794318360,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | The deletion result. The value depends on the deletion method:
- by `ID` — `true` if at least one notification is deleted, `false` if the notification is not found or its recipient is another user
- by `TAG` — always `true`, even if there are no notifications with this tag
- by `SUB_TAG` — `true` if at least one notification is deleted, `false` if there are no notifications with this tag ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ACCESS_DENIED",
    "error_description": "Access denied! Client ID not specified"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Status** | **Code** | **Description** | **Value** ||
|| `403` | `ACCESS_DENIED` | Access denied! Client ID not specified | A webhook call without `ID` and without `CLIENT_ID`, including an empty request ||
|| `400` | `PARAMS_ERROR` | Incorrect params | None of the parameters `ID`, `TAG`, and `SUB_TAG` is passed, or the request was made with session authorization without `ID` ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read-list.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-history-search.md)