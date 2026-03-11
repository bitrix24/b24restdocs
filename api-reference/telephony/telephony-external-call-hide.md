# Hide Call Card for User telephony.externalCall.hide

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalCall.hide` hides the call card for a user or a list of users.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID***
[`string`](../data-types.md) | Identifier of the call from the method [telephony.externalCall.register](./telephony-external-call-register.md). ||
|| **USER_ID**
[`integer/array`](../data-types.md) | Identifier of the user or an array of user identifiers.

Use the identifiers specified in the method [telephony.externalCall.show](./telephony-external-call-show.md)  ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","USER_ID":1269}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/telephony.externalCall.hide
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","USER_ID":1269,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalCall.hide
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalCall.hide',
            {
                CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                USER_ID: 1269
            }
        );
        
        const result = response.getData().result;
        console.log('Call hidden:', result);
        processResult(result);
    }
    catch( error )
    {
        console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'telephony.externalCall.hide',
                [
                    'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                    'USER_ID' => 1269
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error hiding call: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalCall.hide",
        {
            CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
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
        'telephony.externalCall.hide',
        [
            'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            'USER_ID' => 1269
        ]
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP Status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1773131958,
        "finish": 1773131958.809283,
        "duration": 0.8092830181121826,
        "processing": 0,
        "date_start": "2026-03-10T11:39:18+01:00",
        "date_finish": "2026-03-10T11:39:18+01:00",
        "operating_reset_at": 1773132558,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | `true` — call card hidden, `false` — call not found ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method does not have specific error codes. If `CALL_ID` is not found, the method will return `result = false`.

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-call-register.md)
- [{#T}](./telephony-external-call-show.md)
- [{#T}](./telephony-external-call-finish.md)