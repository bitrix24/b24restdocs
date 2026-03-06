# Delete Notification im.notify.delete

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.delete` removes a notification by `ID` or by tags `TAG` and `SUB_TAG`.

{% note warning "" %}

You must provide one of three parameters: `ID`, `TAG`, or `SUB_TAG`.

{% endnote %}

## Method Parameters

{% include [Parameter Note](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **ID***
[`integer`](../../data-types.md) | Identifier of the notification to be deleted ||
|| **TAG***
[`string`](../../data-types.md) | Unique tag of the notification within the application. Only notifications from the current application are deleted ||
|| **SUB_TAG***
[`string`](../../data-types.md) | Additional tag of the notification. Only notifications from the current application are deleted ||
|| **CLIENT_ID**
[`string`](../../data-types.md) | This parameter is required only for webhooks. Pass the same `CLIENT_ID` that was specified during the chat bot registration ||
|#

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

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify.delete', {
        ID: 101,
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
            'im.notify.delete',
            [
                'ID' => 101,
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
[`boolean`](../../data-types.md) | Returns the result of the notification deletion ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "PARAMS_ERROR",
    "error_description": "Incorrect params"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `PARAMS_ERROR` | Incorrect params | `ID`, `TAG`, or `SUB_TAG` not provided, or the request was made with session authorization without `ID` ||
|| `ACCESS_DENIED` | Access denied! Client ID not specified | Unable to determine the application: missing `clientId` authorization and `CLIENT_ID` not provided ||
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