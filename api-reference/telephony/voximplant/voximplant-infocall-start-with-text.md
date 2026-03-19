# Initiate an Auto-Call with the Voice Automation Rule voximplant.infocall.startwithtext

> Scope: [`telephony`](../../scopes/permissions.md), [`call`](../../scopes/permissions.md) 
>
> Who can execute the method: a user with Outgoing Call — Execute access permission

The method `voximplant.infocall.startwithtext` initiates an auto-call and plays the specified text to the recipient using speech synthesis.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FROM_LINE***
[`string`](../../data-types.md) | Identifier of the outgoing line from which the call is initiated.

A list of available lines can be obtained using the [voximplant.line.get](./lines/voximplant-line-get.md) method. ||
|| **TO_NUMBER***
[`string`](../../data-types.md) | The number to which the call should be made. ||
|| **TEXT_TO_PRONOUNCE***
[`string`](../../data-types.md) | The text that will be spoken to the recipient. ||
|| **VOICE**
[`string`](../../data-types.md) | Identifier of the voice for speech synthesis.

If not provided, the default voice for the portal's language will be used.

A list of available voices can be obtained using the [voximplant.tts.voices.get](./voximplant-tts-voices-get.md) method. ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FROM_LINE":"reg151083","TO_NUMBER":"+19991234567","TEXT_TO_PRONOUNCE":"Good afternoon. This is a reminder for your appointment."}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.infocall.startwithtext
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FROM_LINE":"reg151083","TO_NUMBER":"+19991234567","TEXT_TO_PRONOUNCE":"Good afternoon. This is a reminder for your appointment.","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.infocall.startwithtext
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.infocall.startwithtext',
            {
                FROM_LINE: 'reg151083',
                TO_NUMBER: '+19991234567',
                TEXT_TO_PRONOUNCE: 'Good afternoon. This is a reminder for your appointment.'
            }
        );

        const result = response.getData().result;
        console.log('Data:', result);
    }
    catch (error)
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
                'voximplant.infocall.startwithtext',
                [
                    'FROM_LINE' => 'reg151083',
                    'TO_NUMBER' => '+19991234567',
                    'TEXT_TO_PRONOUNCE' => 'Good afternoon. This is a reminder for your appointment.',
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
        'voximplant.infocall.startwithtext',
        {
            FROM_LINE: 'reg151083',
            TO_NUMBER: '+19991234567',
            TEXT_TO_PRONOUNCE: 'Good afternoon. This is a reminder for your appointment.'
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
        'voximplant.infocall.startwithtext',
        [
            'FROM_LINE' => 'reg151083',
            'TO_NUMBER' => '+19991234567',
            'TEXT_TO_PRONOUNCE' => 'Good afternoon. This is a reminder for your appointment.',
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
    "result": {
        "RESULT": true,
        "CALL_ID": "infocall.4aa8f8e35bb6ece352fec0537b58ac42.1773740701"
    },
    "time": {
        "start": 1773740709,
        "finish": 1773740710.094622,
        "duration": 1.0946218967437744,
        "processing": 1,
        "date_start": "2026-03-17T12:45:09+01:00",
        "date_finish": "2026-03-17T12:45:10+01:00",
        "operating_reset_at": 1773741309,
        "operating": 0.14156007766723633
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Object containing the result of the auto-call initiation. ||
|| **RESULT**
[`boolean`](../../data-types.md) | Indicator of successful initiation.

`true` — auto-call successfully initiated. ||
|| **CALL_ID**
[`string`](../../data-types.md) | Identifier of the call. ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

```json
{
    "error": "ERROR_CORE",
    "error_description": "Infocall limit for this month is exceeded"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | `Making infocall using LINK_BASE_NUMBER is not allowed` | Auto-call cannot be initiated through a service line. ||
|| `ERROR_CORE` | `Could not find config for number <LINE_ID>` | Configuration for the specified line not found. ||
|| `ERROR_CORE` | `Infocall limit for this month is exceeded` | Monthly limit for auto-calls exceeded. ||
|| `ERROR_CORE` | `Phone number is not correct` | Invalid number in `TO_NUMBER`. ||
|| `ERROR_CORE` | `Infocall failure` | Error while initiating the auto-call. ||
|| `ACCESS_DENIED` | `Access denied` | Insufficient permissions to make outgoing calls. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-callback-start.md)
- [{#T}](./voximplant-infocall-start-with-sound.md)
- [{#T}](./voximplant-infocall-start-with-text.md)
- [{#T}](./voximplant-tts-voices-get.md)