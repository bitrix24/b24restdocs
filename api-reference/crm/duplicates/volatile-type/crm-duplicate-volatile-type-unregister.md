# Remove Field from Duplicate Search crm.duplicate.volatileType.unregister

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.duplicate.volatileType.unregister` removes a custom field from the duplicate search.

## Method Parameters

{% include [Footnote on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../data-types.md) | Identifier of the field record to be removed. You can obtain the identifiers of records added to the duplicate search using the method [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md) ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.duplicate.volatileType.unregister",
        {
            id: 101
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":101,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.duplicate.volatileType.unregister
    ```

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"id":101}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.duplicate.volatileType.unregister
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.duplicate.volatileType.unregister',
        [
            'id' => 101
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
    "result": true,
    "time": {
        "start": 1750935024.602893,
        "finish": 1750935024.719883,
        "duration": 0.1169898509979248,
        "processing": 0.05846285820007324,
        "date_start": "2025-06-26T13:50:24+02:00",
        "date_finish": "2025-06-26T13:50:24+02:00",
        "operating_reset_at": 1750935624,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "TYPE_IS_NOT_ASSIGNED",
    "error_description": "This type is not assigned."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400` | `TYPE_IS_NOT_ASSIGNED` | Identifier of the added field record not found ||
|#

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md)
- [crm.duplicate.volatileType.list](./crm-duplicate-volatile-type-list.md)
- [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md)