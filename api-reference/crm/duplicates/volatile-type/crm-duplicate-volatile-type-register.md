# Add a Field to the Duplicate Search crm.duplicate.volatileType.register

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.duplicate.volatileType.register` adds a field to the duplicate search functionality for leads, contacts, or companies.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **entityTypeId*** 
[`integer`](../../../data-types.md) | Identifier for the object type. Possible values:
- `1` — [lead](../../leads/index.md)
- `3` — [contact](../../contacts/index.md)
- `4` — [company](../../companies/index.md) ||
|| **fieldCode*** 
[`string`](../../../data-types.md) | The code of the field to be added to the duplicate search. For example, `TITLE`, `RQ.DE.NAME`, `UF_CRM_1750854801`. You can obtain a list of available fields using the method [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md) ||
|#

### Method Operation Features

A total of 7 custom fields can be registered for duplicate searches. For example, if you have already added 3 fields for contacts and 4 fields for companies, attempting to add another field for any object type will result in the error `MAX_TYPES_COUNT_EXCEEDED`.

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.duplicate.volatileType.register",
        {
            entityTypeId: 1,
            fieldCode: "TITLE"
        },
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
         -d '{"entityTypeId":1,"fieldCode":"TITLE"}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.duplicate.volatileType.register
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"entityTypeId":1,"fieldCode":"TITLE","auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.duplicate.volatileType.register
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.duplicate.volatileType.register',
        [
            'entityTypeId' => 1,
            'fieldCode' => 'TITLE'
        ]
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
        "id": 3355
    },
    "time": {
        "start": 1750934251.736599,
        "finish": 1750934252.028757,
        "duration": 0.2921581268310547,
        "processing": 0.24904417991638184,
        "date_start": "2025-06-26T13:37:31+02:00",
        "date_finish": "2025-06-26T13:37:32+02:00",
        "operating_reset_at": 1750934851,
        "operating": 0.24902796745300293
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier for the record of the field added to the duplicate search ||
|| **time**[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": "Field not found",
    "error_description": "The specified field was not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `Field not found` | The specified field was not found ||
|| `400` | `MAX_TYPES_COUNT_EXCEEDED` | The maximum number of custom field types in the duplicate search has been exceeded ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md)
- [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md)
- [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md)