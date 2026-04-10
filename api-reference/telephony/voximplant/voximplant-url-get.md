# Get Links for Navigation in the Telephony Pages voximplant.url.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `voximplant.url.get` returns links for navigation in the telephony pages.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/voximplant.url.get
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/voximplant.url.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'voximplant.url.get',
            {}
        );
        
        const result = response.getData().result;
        console.log('Data:', result);
        
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
                'voximplant.url.get',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'voximplant.url.get',
        {},
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
        'voximplant.url.get',
        []
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
        "detail_statistics": "https://test.bitrix24.com/telephony/detail.php",
        "buy_connector": "https://test.bitrix24.com/settings/license_phone_sip.php",
        "edit_config": "https://test.bitrix24.com/telephony/edit.php?ID=#CONFIG_ID#",
        "lines": "https://test.bitrix24.com/telephony/lines.php"
    },
    "time": {
        "start": 1773323182,
        "finish": 1773323182.857974,
        "duration": 0.8579740524291992,
        "processing": 0,
        "date_start": "2026-03-12T16:46:22+01:00",
        "date_finish": "2026-03-12T16:46:22+01:00",
        "operating_reset_at": 1773323782,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | An object containing links to telephony pages ||
|| **detail_statistics**
[`string`](../../data-types.md) | Link to the detailed statistics page ||
|| **buy_connector**
[`string`](../../data-types.md) | Link to the page for purchasing the SIP connector ||
|| **edit_config**
[`string`](../../data-types.md) | Link to the page for editing the connected line. Replace `#CONFIG_ID#` with the configuration ID ||
|| **lines**
[`string`](../../data-types.md) | Link to the list of lines page ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./voximplant-callback-start.md)
- [{#T}](./voximplant-infocall-start-with-sound.md)
- [{#T}](./voximplant-infocall-start-with-text.md)
- [{#T}](./voximplant-tts-voices-get.md)