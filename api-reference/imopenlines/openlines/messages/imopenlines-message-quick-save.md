# Save Message as Quick Answer imopenlines.message.quick.save

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator or open line operator

The method `imopenlines.message.quick.save` saves a message from the open line chat to the list of quick answers.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Description** ||
|| **CHAT_ID***
[`integer`](../../../data-types.md) | Identifier of the open line chat from which the message needs to be saved.

The chat identifier can be obtained using the [imopenlines.dialog.get](../sessions/imopenlines-dialog-get.md) or [imopenlines.session.open](../sessions/imopenlines-session-open.md) methods. ||
|| **MESSAGE_ID***
[`integer`](../../../data-types.md) | Identifier of the message to be added to quick answers.

The message identifier can be obtained using the [imopenlines.session.history.get](../sessions/imopenlines-session-history-get.md) method. ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":1331,"MESSAGE_ID":83507}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/imopenlines.message.quick.save
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CHAT_ID":1331,"MESSAGE_ID":83507,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/imopenlines.message.quick.save
    ```

- JS

    ```js
    try {
      const response = await $b24.callMethod(
        'imopenlines.message.quick.save',
        {
          CHAT_ID: 1331,
          MESSAGE_ID: 83507,
        }
      );

      const { result } = response.getData();
      console.log('Saved to quick answers:', result);
    } catch (error) {
      console.error('Error saving quick answer:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'imopenlines.message.quick.save',
                [
                    'CHAT_ID'    => 1331,
                    'MESSAGE_ID' => 83507,
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        if ($result->error()) {
            echo 'Error: ' . $result->error();
        } else {
            echo 'Saved to quick answers: ' . var_export($result->data(), true);
        }
    } catch (Throwable $exception) {
        error_log($exception->getMessage());
        echo 'Error saving quick answer: ' . $exception->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'imopenlines.message.quick.save',
        {
            CHAT_ID: 1331,
            MESSAGE_ID: 83507,
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
        'imopenlines.message.quick.save',
        [
            'CHAT_ID'    => 1331,
            'MESSAGE_ID' => 83507,
        ]
    );

    if (!empty($result['error'])) {
        echo 'Error: ' . $result['error_description'];
    } else {
        echo 'Saved to quick answers: ' . var_export($result['result'], true);
    }
    ```

{% endlist %}

## Response Handling

HTTP Code: **200**

```json
{
    "result": true,
    "time": {
        "start": 1728626400.456,
        "finish": 1728626400.539,
        "duration": 0.083,
        "processing": 0.031,
        "date_start": "2024-10-11T10:00:00+02:00",
        "date_finish": "2024-10-11T10:00:00+02:00",
        "operating": 0
    }
}
```

## Returned Data

#|
|| **Name**
`Type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Contains `true` when successfully saved to quick answers. ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "CANT_SAVE_QUICK_ANSWER",
    "error_description": "Error saving quick answer"
}
```

{% include notitle [Error Handling](../../../../_includes/error-info.md) %}

### Possible Errors

#|
|| **Code** | **Description** ||
|| `CHAT_ID` | Invalid chat identifier provided. ||
|| `CHAT_TYPE` | The specified chat is not an open line. ||
|| `CANT_SAVE_QUICK_ANSWER` | Error saving quick answer. ||
|| `ACCESS_DENIED` | Insufficient rights to open the specified chat. ||
|#

{% include [System Errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./imopenlines-crm-message-add.md)