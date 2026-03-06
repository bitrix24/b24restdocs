# Set User Status im.user.status.set

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.user.status.set` sets the current user's status.

To check the current status, use the method [im.user.status.get](./im-user-status-get.md).

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **STATUS*** 
[`string`](../../data-types.md) | The new status of the user. The value is case-sensitive and must be provided in lowercase.

Allowed values: 
- `online` — online, the user receives all notifications
- `dnd` — do not disturb, this status disables notifications
- `away` — away
- `break` — on break ||
|#

{% note info "" %}

In the new messenger interface, only the `online` status is displayed. The statuses `dnd`, `away`, and `break` can be set using the `im.user.status.set` method, but they are not shown in the interface.

[Bitrix24 Chat: New Messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}


## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"STATUS":"dnd"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.user.status.set
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"STATUS":"dnd","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.user.status.set
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.user.status.set', {
        STATUS: 'dnd',
      });

      const { result } = response.getData();
      console.log(result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.user.status.set',
            [
                'STATUS' => 'dnd',
            ]
        );

        $result = $response->getResponseData()->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            var_dump($result->data());
        }
    } catch (Throwable $exception) {
        echo $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'im.user.status.set',
        {
            STATUS: 'dnd',
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
        'im.user.status.set',
        [
            'STATUS' => 'dnd',
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        var_dump($result['result']);
    }
    ```
{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1760000000.0,
        "finish": 1760000000.1,
        "duration": 0.1,
        "processing": 0.04,
        "date_start": "2026-03-02T09:30:00+01:00",
        "date_finish": "2026-03-02T09:30:00+01:00",
        "operating_reset_at": 1760030000,
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Returns `true` if the status is set ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "STATUS_ERROR",
    "error_description": "Status is not available"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `STATUS_ERROR` | Status is not available | The provided `STATUS` value is not in the list of available statuses ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-user-get.md)
- [{#T}](./im-user-list-get.md)
- [{#T}](./im-user-status-get.md)
- [{#T}](./im-user-status-idle-start.md)
- [{#T}](./im-user-status-idle-end.md)