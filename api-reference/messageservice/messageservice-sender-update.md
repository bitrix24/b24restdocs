# Update SMS provider messageservice.sender.update

> Scope: [`messageservice`](../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the SMS provider.

## Method Parameters

{% include [Note on required parameters](../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **CODE***
[`string`](../data-types.md) | Internal identifier of the provider ||
|| **HANDLER**
[`string`](../data-types.md) | URL of the application to which the data will be sent ||
|| **NAME**
[`string / array`](../data-types.md) | Name of the provider. It can be a string or an associative array of localized strings. 

This parameter is required if the phrase is in a new language ||
|| **DESCRIPTION**
[`string / array`](../data-types.md) | Description of the provider. It can be a string or an associative array of localized strings. 

Used only with the `NAME` parameter if the language is new ||
|#

The request must contain at least one optional parameter.

## Code Examples

{% include [Note on examples](../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider","HANDLER":"https://newhandler.com/","NAME":"New Provider Name","DESCRIPTION":"New Provider Description"}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/messageservice.sender.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider","HANDLER":"https://newhandler.com/","NAME":"New Provider Name","DESCRIPTION":"New Provider Description","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.sender.update
    ```

- JS

    ```js
    var params = {
        CODE: 'provider',
        HANDLER: 'https://newhandler.com/',
        NAME: 'New Provider Name',
        DESCRIPTION: 'New Provider Description'
    };
    BX24.callMethod(
        'messageservice.sender.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $params = [
        'CODE' => 'provider',
        'HANDLER' => 'https://newhandler.com/',
        'NAME' => 'New Provider Name',
        'DESCRIPTION' => 'New Provider Description'
    ];

    $result = CRest::call(
        'messageservice.sender.update',
        $params
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider","NAME":{"en":"New Name","de":"Neuer Name"},"DESCRIPTION":{"en":"New Description"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/messageservice.sender.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"CODE":"provider","NAME":{"en":"New Name","de":"Neuer Name"},"DESCRIPTION":{"en":"New Description"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/messageservice.sender.update
    ```

- JS

    ```js
    var params = {
        CODE: 'provider',
        NAME: {"en":"New Name","de":"Neuer Name"},
        DESCRIPTION: {"en":"New Description"}
    };
    BX24.callMethod(
        'messageservice.sender.update',
        params,
        function(result)
        {
            if(result.error())
                alert("Error: " + result.error());
            else
                alert("Success: " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $params = [
        'CODE' => 'provider',
        'NAME' => [
            'en' => 'New Name',
            'de' => 'Neuer Name'
        ],
        'DESCRIPTION' => [
            'en' => 'New Description'
        ]
    ];

    $result = CRest::call(
        'messageservice.sender.update',
        $params
    );

    echo '<PRE>';
    print_r($result);
    echo '</PRE>';
    ```

{% endlist %}

## Response Handling

HTTP status: **200**

```json
{
    "result": true,
    "time": {
        "start": 1732110540.526103,
        "finish": 1732110540.797043,
        "duration": 0.27094006538391113,
        "processing": 0.007060050964355469,
        "date_start": "2024-11-20T15:49:00+02:00",
        "date_finish": "2024-11-20T15:49:00+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../data-types.md) | Result of the SMS provider update ||
|| **time**
[`time`](../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**, **403**

```json
{
    "error": "ERROR_SENDER_NOT_FOUND",
    "error_description": "Sender not found!"
}
```

{% include notitle [error handling](../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `ERROR_SENDER_NOT_FOUND` | Provider not found ||
|| `ERROR_SENDER_CODE_REQUIRED` | The `CODE` parameter is missing ||
|| `ERROR_SENDER_OTHER_PARAMS_REQUIRED` | At least one of the optional parameters is missing ||
|| `ACCESS_DENIED` | Insufficient permissions to update the SMS provider ||
|#

{% include [system errors](../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./messageservice-sender-add.md)
- [{#T}](./messageservice-sender-list.md)
- [{#T}](./messageservice-sender-delete.md)
- [{#T}](./messageservice-message-status-update.md)