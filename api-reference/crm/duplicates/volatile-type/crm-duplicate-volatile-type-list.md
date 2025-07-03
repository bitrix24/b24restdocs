# Get a list of custom fields involved in duplicate search crm.duplicate.volatileType.list

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

The method `crm.duplicate.volatileType.list` returns a list of custom fields that are already used for duplicate searches in leads, contacts, and companies.

## Method Parameters

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "crm.duplicate.volatileType.list",
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
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.duplicate.volatileType.list
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.duplicate.volatileType.list
    ```   

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.duplicate.volatileType.list',
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
    "result": [
        {
            "id": 3355,
            "entityTypeId": 1,
            "fieldCode": "TITLE"
        }
    ],
    "time": {
        "start": 1750934651.513455,
        "finish": 1750934651.578262,
        "duration": 0.06480717658996582,
        "processing": 0.017321109771728516,
        "date_start": "2025-06-26T13:44:11+02:00",
        "date_finish": "2025-06-26T13:44:11+02:00",
        "operating_reset_at": 1750935251,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **id**
[`integer`](../../../data-types.md) | Record identifier ||
|| **entityTypeId**
[`integer`](../../../data-types.md) | Object type ||
|| **fieldCode**
[`string`](../../../data-types.md) | Field code ||
|| **time**[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

The method does not return errors.

{% include [system errors](./../../../../_includes/system-errors.md) %}

## Continue Learning

- [crm.duplicate.volatileType.fields](./crm-duplicate-volatile-type-fields.md)
- [crm.duplicate.volatileType.register](./crm-duplicate-volatile-type-register.md)
- [crm.duplicate.volatileType.unregister](./crm-duplicate-volatile-type-unregister.md)