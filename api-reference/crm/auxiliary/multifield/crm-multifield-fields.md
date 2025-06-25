# Get Description of Multiple Fields crm.multifield.fields

> Scope: [`crm`](../../../scopes/permissions.md)
> 
> Who can execute the method: any user

The method `crm.multifield.fields` returns the description of multiple fields used to store phone numbers, email addresses, and other contact information in leads, contacts, and companies.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.multifield.fields",
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
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.multifield.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"auth":"**put_access_token_here**"}' \
         https://**put_your_bitrix24_address**/rest/crm.multifield.fields
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.multifield.fields',
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
            "type": "int",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "TYPE_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Field Type"
        },
        "VALUE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Value"
        },
        "VALUE_TYPE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Value Type"
        }
    },
    "time": {
        "start": 1750684622.069806,
        "finish": 1750684622.120529,
        "duration": 0.05072283744812012,
        "processing": 0.00037217140197753906,
        "date_start": "2025-06-23T16:17:02+02:00",
        "date_finish": "2025-06-23T16:17:02+02:00",
        "operating_reset_at": 1750685222,
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

#### Fields of the result Object {#result}

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier ||
|| **TYPE_ID**
[`string`](../../../data-types.md) | Field Type ||
|| **VALUE**
[`string`](../../../data-types.md) | Value ||
|| **VALUE_TYPE**
[`string`](../../../data-types.md) | Value Type ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](../index.md)