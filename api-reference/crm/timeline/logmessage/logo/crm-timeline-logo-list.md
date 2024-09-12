# Get a List of Available Logos crm.timeline.logo.list

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: `any user`

This method retrieves a list of available logos for timeline log entries.

No parameters.

## Code Examples

{% include [Footnote on examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.timeline.logo.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.timeline.logo.list
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.timeline.logo.list", 
        {},
        result => {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.timeline.logo.list',
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
        "logos": [
            {
                "code": "call",
                "isSystem": true,
                "fileUri": ""
            },
            {
                "code": "arrow-down",
                "isSystem": true,
                "fileUri": ""
            },
            {
                "code": "info",
                "isSystem": false,
                "fileUri": "/upload/crm/286/a1k092hygdrzpiz6a01enuymqoo5qzym/ou0akdwnbxalzk9hgfme39nbvtozblew"
            }
        ]
    },
    "total": 3,
    "time": {
        "start": 1712132792.910734,
        "finish": 1712132793.530359,
        "duration": 0.6196250915527344,
        "processing": 0.032338857650756836,
        "date_start": "2024-04-03T10:26:32+02:00",
        "date_finish": "2024-04-03T10:26:33+02:00",
        "operating_reset_at": 1705765533,
        "operating": 3.3076241016387939
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | Root element of the response.

The `result` field contains an array of `logos`, each entry of which includes an associative array of logo fields [logo](./crm-timeline-logo-add.md#logo) ||
|| **total**
[`integer`](../../../../data-types.md) | Total number of records found ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "0",
    "error_description": "Could not find value"
}
```

{% include notitle [error handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `0` | Other errors (e.g., fatal) ||
|#

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./crm-timeline-logo-add.md)
- [{#T}](./crm-timeline-logo-get.md)
- [{#T}](./crm-timeline-logo-delete.md)