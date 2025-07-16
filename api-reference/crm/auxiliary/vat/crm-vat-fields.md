# Get VAT Rate Fields crm.vat.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `crm.vat.fields` returns the description of VAT rate fields.

## Method Parameters

No parameters.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.vat.fields",
        {},
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.vat.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.vat.fields
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.vat.fields',
        []
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
    "result": {
        "ID": {
            "type": "integer",
            "size": "11",
            "isRequired": false,
            "isReadOnly": true,
            "title": "ID"
        },
        "TIMESTAMP_X": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "title": "Modification Date"
        },
        "ACTIVE": {
            "type": "string",
            "size": "1",
            "isRequired": false,
            "isReadOnly": false,
            "title": "Activity"
        },
        "C_SORT": {
            "type": "integer",
            "size": "18",
            "isRequired": false,
            "isReadOnly": false,
            "title": "Sorting"
        },
        "NAME": {
            "type": "string",
            "size": "50",
            "isRequired": false,
            "isReadOnly": false,
            "title": "Name"
        },
        "RATE": {
            "type": "double",
            "size": "18,2",
            "isRequired": false,
            "isReadOnly": false,
            "title": "Rate"
        }
    },
    "time": {
        "start": 1751984389.802849,
        "finish": 1751984389.832249,
        "duration": 0.029399871826171875,
        "processing": 0.0001289844512939453,
        "date_start": "2025-07-08T17:19:49+02:00",
        "date_finish": "2025-07-08T17:19:49+02:00",
        "operating_reset_at": 1751984989,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Object with field descriptions [(detailed description)](#result) ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

#### Fields of the result object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID** 
[`integer`](../../../data-types.md) | Identifier of the VAT rate ||
|| **TIMESTAMP_X** 
[`datetime`](../../../data-types.md) | Modification date ||
|| **ACTIVE**
[`string`](../../../data-types.md) | Activity of the rate ||
|| **C_SORT**
[`integer`](../../../data-types.md) | Sorting ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the rate ||
|| **RATE**
[`double`](../../../data-types.md) | Value of the VAT rate, % ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-vat-list.md)
- [{#T}](./crm-vat-get.md)
- [{#T}](./crm-vat-add.md)
- [{#T}](./crm-vat-update.md)
- [{#T}](./crm-vat-delete.md) 