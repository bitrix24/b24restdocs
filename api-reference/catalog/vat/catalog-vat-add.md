# Add VAT Rate catalog.vat.add

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method adds a new VAT rate.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a new VAT rate ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **name***
[`string`](../../data-types.md) | Name of the VAT rate ||
|| **active**
[`string`](../../data-types.md) | Indicator of the VAT rate's activity. Possible values:
- `Y` — active
- `N` — inactive

Default is `Y`
||
|| **rate***
[`double`](../../data-types.md) | Value of the VAT rate ||
|| **sort**
[`integer`](../../data-types.md) | Sorting.

Default is `100`
||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Tax 13%","rate":13,"sort":10,"active":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"name":"Tax 13%","rate":13,"sort":10,"active":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.vat.add
    ```

- JS

    ```js
    BX24.callMethod(
    'catalog.vat.add',
            {
                fields: {
                        name: 'Tax 13%',
                        rate: 13,
                        sort: 10,
                        active: "Y",
                }
            },
    function(result) {
            if (result.error())
                console.error(result.error());
            else
                console.log(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.vat.add',
        [
            'fields' => [
                'name' => 'Tax 13%',
                'rate' => 13,
                'sort' => 10,
                'active' => "Y"
            ]
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
        "vat": {
            "active": "Y",
            "id": 5,
            "name": "Tax 13%",
            "rate": 13,
            "sort": 10,
            "timestampX": "2024-09-16T11:20:38+02:00"
        }
    },
    "time": {
        "start": 1716552521.40908,
        "finish": 1716552521.69852,
        "duration": 0.289434909820557,
        "processing": 0.011207103729248,
        "date_start": "2024-09-16T11:20:38+02:00",
        "date_finish": "2024-09-16T11:20:38+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../data-types.md) | Root element of the response ||
|| **vat**
[`catalog_vat`](../data-types.md#catalog_vat) | Object with information about the created VAT rate
||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200040300020,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `100` | Required parameter `fields` not provided
||
|| `0` | Required fields not set
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-vat-update.md)
- [{#T}](./catalog-vat-get.md)
- [{#T}](./catalog-vat-list.md)
- [{#T}](./catalog-vat-delete.md)
- [{#T}](./catalog-vat-get-fields.md)