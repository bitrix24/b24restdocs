# Read the list of notifications im.notify.read.list

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.read.list` marks a list of notifications as read or unread.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **IDS***
[`array`](../../data-types.md) | An array of notification identifiers. If any value in the array is `<= 0`, processing stops at that element ||
|| **ACTION**
[`string`](../../data-types.md) | Action on the notifications:
- `Y` — mark as read
- `N` — mark as unread

The default value is `Y`.

If any value other than `Y` is provided, the method applies the action as for `N` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"IDS":[101,102,103],"ACTION":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.read.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"IDS":[101,102,103],"ACTION":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.read.list
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify.read.list', {
        IDS: [101, 102, 103],
        ACTION: 'Y',
      });
      const { result } = response.getData();
      console.log('Result:', result);
    } catch (error) {
      console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service->core->call(
            'im.notify.read.list',
            [
                'IDS' => [101, 102, 103],
                'ACTION' => 'Y',
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
        'im.notify.read.list',
        {
            IDS: [101, 102, 103],
            ACTION: 'Y',
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
        'im.notify.read.list',
        [
            'IDS' => [101, 102, 103],
            'ACTION' => 'Y',
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
[`boolean`](../../data-types.md) | Returns `true` after processing the list of notifications ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "No IDS param or it is not an array"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `PARAMS_ERROR` | No IDS param or it is not an array | The `IDS` parameter is not provided or is not an array ||
|#

{% include [System Errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./im-notify.md)
- [{#T}](./im-notify-personal-add.md)
- [{#T}](./im-notify-system-add.md)
- [{#T}](./im-notify-get.md)
- [{#T}](./im-notify-schema-get.md)
- [{#T}](./im-notify-read.md)
- [{#T}](./im-notify-read-all.md)
- [{#T}](./im-notify-answer.md)
- [{#T}](./im-notify-confirm.md)
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)