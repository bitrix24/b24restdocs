# Add Transcription to Call with telephony.call.attachTranscription

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.call.attachTranscription` adds a transcription of the conversation to a completed call.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CALL_ID***
[`string`](../data-types.md) | Identifier of the call from the method [telephony.externalCall.register](./telephony-external-call-register.md) ||
|| **MESSAGES***
[`array`](../data-types.md) | Array of [transcription utterances](#messages) ||
|| **COST**
[`double`](../data-types.md) | Cost of the transcription.

By default — not set ||
|| **COST_CURRENCY**
[`string`](../data-types.md) | Currency of the transcription cost. Used only with `COST`.

By default — not set ||
|#

### MESSAGES Parameter {#messages}

#|
|| **Name**
`type` | **Description** ||
|| **SIDE***
[`string`](../data-types.md) | Participant of the conversation.

Possible values:
- `User` — portal user
- `Client` — client ||
|| **START_TIME***
[`integer`](../data-types.md) | Start time of the utterance in seconds from the beginning of the call.

Minimum value — `0` ||
|| **STOP_TIME***
[`integer`](../data-types.md) | End time of the utterance in seconds from the beginning of the call.

Minimum value — `1` ||
|| **MESSAGE***
[`string`](../data-types.md) | Text of the utterance ||
|#

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","MESSAGES":[{"SIDE":"User","START_TIME":1,"STOP_TIME":3,"MESSAGE":"Good afternoon"},{"SIDE":"Client","START_TIME":4,"STOP_TIME":7,"MESSAGE":"Hello"}]}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/telephony.call.attachTranscription
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CALL_ID":"externalCall.716f1cb73def9700a23842adf9c4c568.1773130779","MESSAGES":[{"SIDE":"User","START_TIME":1,"STOP_TIME":3,"MESSAGE":"Good afternoon"},{"SIDE":"Client","START_TIME":4,"STOP_TIME":7,"MESSAGE":"Hello"}],"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.call.attachTranscription
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.call.attachTranscription',
            {
                CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                MESSAGES: [
                    { SIDE: 'User', START_TIME: 1, STOP_TIME: 3, MESSAGE: 'Good afternoon' },
                    { SIDE: 'Client', START_TIME: 4, STOP_TIME: 7, MESSAGE: 'Hello' }
                ]
            }
        );
        
        const result = response.getData().result;
        console.log('Transcription attached:', result);
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
                'telephony.call.attachTranscription',
                [
                    'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
                    'MESSAGES' => [
                        ['SIDE' => 'User', 'START_TIME' => 1, 'STOP_TIME' => 3, 'MESSAGE' => 'Good afternoon'],
                        ['SIDE' => 'Client', 'START_TIME' => 4, 'STOP_TIME' => 7, 'MESSAGE' => 'Hello']
                    ]
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error attaching transcription: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.call.attachTranscription",
        {
            CALL_ID: 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            MESSAGES: [
                { SIDE: 'User', START_TIME: 1, STOP_TIME: 3, MESSAGE: 'Good afternoon' },
                { SIDE: 'Client', START_TIME: 4, STOP_TIME: 7, MESSAGE: 'Hello' }
            ]
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
        'telephony.call.attachTranscription',
        [
            'CALL_ID' => 'externalCall.716f1cb73def9700a23842adf9c4c568.1773130779',
            'MESSAGES' => [
                ['SIDE' => 'User', 'START_TIME' => 1, 'STOP_TIME' => 3, 'MESSAGE' => 'Good afternoon'],
                ['SIDE' => 'Client', 'START_TIME' => 4, 'STOP_TIME' => 7, 'MESSAGE' => 'Hello']
            ]
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
        "TRANSCRIPT_ID": 1
    },
    "time": {
        "start": 1773136191,
        "finish": 1773136191.49517,
        "duration": 0.49517011642456055,
        "processing": 0,
        "date_start": "2026-03-10T12:49:51+01:00",
        "date_finish": "2026-03-10T12:49:51+01:00",
        "operating_reset_at": 1773136791,
        "operating": 0.1380019187927246
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | Root element of the response ||
|| **TRANSCRIPT_ID**
[`integer`](../data-types.md) | Identifier of the transcription ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "ERROR_CORE",
    "error_description": "MESSAGES should be an array"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | CALL_ID should be set | `CALL_ID` not provided ||
|| `ERROR_CORE` | MESSAGES should be an array | Parameter `MESSAGES` not passed as an array ||
|| `ERROR_CORE` | MESSAGES[{N}][SIDE] should be either Client or User | Invalid value for `SIDE` ||
|| `ERROR_CORE` | MESSAGES[{N}][START_TIME] should be greater or equal to zero | Invalid `START_TIME` ||
|| `ERROR_CORE` | MESSAGES[{N}][STOP_TIME] should be greater than zero | Invalid `STOP_TIME` ||
|| `ERROR_CORE` | MESSAGES[{N}][MESSAGE] is empty | Empty text for the utterance ||
|| `ERROR_CORE` | Call {CALL_ID} is not found. Is it finished? | Call not found in the statistics. Ensure the call is completed ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-call-register.md)
- [{#T}](./telephony-external-call-finish.md)
- [{#T}](./telephony-external-call-attach-record.md)
- [{#T}](./voximplant/voximplant-statistic-get.md)