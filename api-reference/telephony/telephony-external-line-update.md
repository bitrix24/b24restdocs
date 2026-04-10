# Update External Line telephony.externalLine.update

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

> Scope: [`telephony`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `telephony.externalLine.update` modifies the parameters of the application's external line.

{% note info "" %}

This method works only in the context of the [application](../../settings/app-installation/index.md).

{% endnote %}

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **NUMBER***
[`string`](../data-types.md) | The number of the external line.

The number can be obtained using the [telephony.externalLine.get](./telephony-external-line-get.md) method. ||
|| **NAME**
[`string`](../data-types.md) | The new name of the external line. ||
|| **CRM_AUTO_CREATE**
[`string`](../data-types.md) | Auto-creation of a CRM object for outgoing calls.

Possible values:

 `Y` — enabled
 `N` — disabled ||
|#

{% note info "" %}

At least one field must be provided for modification: `NAME` or `CRM_AUTO_CREATE`. Otherwise, you will receive an `ERROR_CORE` error.

{% endnote %}

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"NUMBER":"14151234567","NAME":"Support Line","CRM_AUTO_CREATE":"N","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/telephony.externalLine.update
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            'telephony.externalLine.update',
            {
                NUMBER: '14151234567',
                NAME: 'Support Line',
                CRM_AUTO_CREATE: 'N'
            }
        );
        
        const result = response.getData().result;
        console.log('Updated external line with ID:', result);
        
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
                'telephony.externalLine.update',
                [
                    'NUMBER' => '14151234567',
                    'NAME' => 'Support Line',
                    'CRM_AUTO_CREATE' => 'N'
                ]
            );

        $result = $response
            ->getResponseData()
            ->getResult();

        echo 'Success: ' . print_r($result, true);
        processData($result);

    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating external line: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "telephony.externalLine.update",
        {
            NUMBER: '14151234567',
            NAME: 'Support Line',
            CRM_AUTO_CREATE: 'N'
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
        'telephony.externalLine.update',
        [
            'NUMBER' => '14151234567',
            'NAME' => 'Support Line',
            'CRM_AUTO_CREATE' => 'N'
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
        "ID": "7"
    },
    "time": {
        "start": 1772806119,
        "finish": 1772806120.006578,
        "duration": 1.006577968597412,
        "processing": 1,
        "date_start": "2026-03-06T17:08:39+02:00",
        "date_finish": "2026-03-06T17:08:40+02:00",
        "operating_reset_at": 1772806719,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../data-types.md) | The root element of the response. ||
|| **ID**
[`integer`](../data-types.md) | The identifier of the updated external line. ||
|| **time**
[`time`](../data-types.md#time) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **400**, **403**

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
|| `WRONG_AUTH_TYPE` | Current authorization type is denied for this method | Method called outside the context of the application. ||
|| `ERROR_CORE` | There are no fields to update | No fields provided for modification `NAME` or `CRM_AUTO_CREATE`. ||
|| `ERROR_CORE` | NUMBER should not be empty | Required parameter `NUMBER` not provided. ||
|| `ERROR_CORE` | Could not find line with number {NUMBER} | Line with the specified number not found. ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./telephony-external-line-add.md)
- [{#T}](./telephony-external-line-get.md)
- [{#T}](./telephony-external-line-delete.md)
- [{#T}](./telephony-external-call-register.md)