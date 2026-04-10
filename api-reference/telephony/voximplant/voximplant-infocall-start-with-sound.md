# Make an Auto-Call with MP3 Playback using voximplant.infocall.startwithsound

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../scopes/permissions.md), [`call`](../../scopes/permissions.md) 
>
> Who can execute the method: user with Outgoing Call — Execute permission

The method `voximplant.infocall.startwithsound` initiates an auto-call and plays an MP3 file from a URL.

## Method Parameters

{% include [Footnote on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **FROM_LINE***
[`string`](../../data-types.md) | Identifier of the outgoing line from which the call is initiated.

A list of available lines can be obtained using the [voximplant.line.get](./lines/voximplant-line-get.md) method. ||
|| **TO_NUMBER***
[`string`](../../data-types.md) | The number to which the call should be made. ||
|| **URL***
[`string`](../../data-types.md) | Link to the MP3 file that should be played during the call. ||
|#

## Code Examples

{% include [Footnote on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FROM_LINE":"reg151083","TO_NUMBER":"1234567890","URL":"https://example.com/sound/notice.mp3"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.infocall.startwithsound
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"FROM_LINE":"reg151083","TO_NUMBER":"1234567890","URL":"https://example.com/sound/notice.mp3","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.infocall.startwithsound
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.infocall.startwithsound',
            {
                FROM_LINE: 'reg151083',
                TO_NUMBER: '1234567890',
                URL: 'https://example.com/sound/notice.mp3'
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
                'voximplant.infocall.startwithsound',
                [
                    'FROM_LINE' => 'reg151083',
                    'TO_NUMBER' => '1234567890',
                    'URL' => 'https://example.com/sound/notice.mp3',
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
        'voximplant.infocall.startwithsound',
        {
            FROM_LINE: 'reg151083',
            TO_NUMBER: '1234567890',
            URL: 'https://example.com/sound/notice.mp3'
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
        'voximplant.infocall.startwithsound',
        [
            'FROM_LINE' => 'reg151083',
            'TO_NUMBER' => '1234567890',
            'URL' => 'https://example.com/sound/notice.mp3',
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
        "CALL_ID": "infocall.e51f04feaf1b58fb5dc6ee9d03d98fea.1773740145"
    },
    "time": {
        "start": 1773740189,
        "finish": 1773740189.20988,
        "duration": 0.20988011360168457,
        "processing": 0,
        "date_start": "2026-03-17T12:36:29+01:00",
        "date_finish": "2026-03-17T12:36:29+01:00",
        "operating_reset_at": 1773740789,
        "operating": 0.13379693031311035
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
    "error_description": "Could not find config for number reg000000"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `ERROR_CORE` | `Making infocall using LINK_BASE_NUMBER is not allowed` | Auto-call cannot be initiated through a service line. ||
|| `ERROR_CORE` | `Could not find config for number <LINE_ID>` | Configuration for the specified line not found. ||
|| `ERROR_CORE` | `Infocall limit for this month is exceeded` | Monthly limit for auto-calls exceeded. ||
|| `ERROR_CORE` | `Phone number is not correct` | Incorrect number in `TO_NUMBER`. ||
|| `ERROR_CORE` | `Infocall failure` | Error occurred while initiating the auto-call. ||
|| `ACCESS_DENIED` | `Access denied` | Insufficient permissions to make outgoing calls. ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-callback-start.md)
- [{#T}](./voximplant-infocall-start-with-sound.md)
- [{#T}](./voximplant-infocall-start-with-text.md)
- [{#T}](./voximplant-tts-voices-get.md)