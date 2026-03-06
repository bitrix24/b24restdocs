# Interacting with Notification Buttons im.notify.confirm

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `im.notify.confirm` sends the selected value of the notification button.

## Method Parameters

{% include [Footnote on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **NOTIFY_ID***
[`integer`](../../data-types.md) | Identifier of the notification with buttons. 

You can obtain the notification identifier using the [im.notify.get](./im-notify-get.md) method. ||
|| **NOTIFY_VALUE***
[`string`](../../data-types.md) | Value of the selected button. ||
|#

For example, consider a notification that supports buttons:
- Accept — value `'Y'`
- Decline — value `'N'`

![Example of buttons](./_images/buttons_example.png)

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"NOTIFY_ID":288,"NOTIFY_VALUE":"Y"}' \
      https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/im.notify.confirm
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{"NOTIFY_ID":288,"NOTIFY_VALUE":"Y","auth":"**put_access_token_here**"}' \
      https://**put_your_bitrix24_address**/rest/im.notify.confirm
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod('im.notify.confirm', {
        NOTIFY_ID: 288,
        NOTIFY_VALUE: 'Y',
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
            'im.notify.confirm',
            [
                'NOTIFY_ID' => 288,
                'NOTIFY_VALUE' => 'Y',
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
        'im.notify.confirm',
        {
            NOTIFY_ID: 288,
            NOTIFY_VALUE: 'Y',
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
        'im.notify.confirm',
        [
            'NOTIFY_ID' => 288,
            'NOTIFY_VALUE' => 'Y',
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
    "result": {
        "result_message": [
            "Invitation accepted"
        ]
    },
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
[`object`](../../data-types.md) | Object containing the result of the button press. 

The structure of the object is described in detail [below](#result-object) ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

### Result Object {#result-object}

#|
|| **Name**
`Type` | **Description** ||
|| **result_message**
[`array`](../../data-types.md) 
[`boolean`](../../data-types.md) | Response from the notification button handler. May contain an array of strings or `false` ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "NOTIFY_VALUE_ERROR",
    "error_description": "Notification Value can't be empty"
}
```

{% include notitle [Error Handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ID_ERROR` | Notification ID can't be empty | Parameter `NOTIFY_ID` is not provided or `<= 0` ||
|| `NOTIFY_VALUE_ERROR` | Notification Value can't be empty | Empty parameter `NOTIFY_VALUE` ||
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
- [{#T}](./im-notify-delete.md)
- [{#T}](./im-notify-history-search.md)