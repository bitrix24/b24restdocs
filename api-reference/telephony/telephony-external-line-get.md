# Get a List of External Lines with telephony.externalLine.get

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalLine.get` returns a list of external lines for the application.

{% note info "" %}

The method works only in the context of the [application](../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

No parameters.

## Code Examples

{% include [Example Note](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalLine.get
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalLine.get',
            {}
        );
        
        const result = response.getData().result;
        console.log('Retrieved external lines:', result);
        
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
                'telephony.externalLine.get',
                []
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error retrieving external lines: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalLine.get",
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
        'telephony.externalLine.get',
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
    "result": [
        {
        "NUMBER": "14151234567",
        "NAME": "Support Line",
        "CRM_AUTO_CREATE": "N"
        },
        {
        "NUMBER": "14157654321",
        "NAME": "Main External Line",
        "CRM_AUTO_CREATE": "Y"
        }
    ],
    "time": {
        "start": 1772806525,
        "finish": 1772806525.042094,
        "duration": 0.04209399223327637,
        "processing": 0,
        "date_start": "2026-03-06T17:15:25+01:00",
        "date_finish": "2026-03-06T17:15:25+01:00",
        "operating_reset_at": 1772807125,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`array`](../data-types.md) | Array of external lines for the application ||
|| **NUMBER**
[`string`](../data-types.md) | External line number ||
|| **NAME**
[`string`](../data-types.md) | Name of the external line ||
|| **CRM_AUTO_CREATE**
[`string`](../data-types.md) | Flag for automatic CRM object creation ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **403**

```json
{
    "error": "WRONG_AUTH_TYPE",
    "error_description": "Current authorization type is denied for this method"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Method called outside the context of the application ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-line-add.md)
- [{#T}](./telephony-external-line-update.md)
- [{#T}](./telephony-external-line-delete.md)
- [{#T}](./telephony-external-call-register.md)