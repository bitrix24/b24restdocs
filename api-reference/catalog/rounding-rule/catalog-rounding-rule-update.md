# Update Price Rounding Rule catalog.roundingRule.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the price rounding rule.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **Id***
[`catalog_rounding_rule.id`](../data-types.md#catalog_rounding_rule)| Identifier of the price rounding rule ||
|| **fields***
[`object`](../../data-types.md)| Field values for updating the price rounding rule ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId***
[`catalog_price_type.id`](../data-types.md#catalog_price_type) | Price type ||
|| **price***
[`double`](../../data-types.md) | Minimum price for rounding ||
|| **roundType***
[`integer`](../../data-types.md) | Rounding type. Possible values:
- `1` — mathematical rounding
- `2` — rounding up (in favor of the store)
- `4` — rounding down (in favor of the customer)
||
|| **roundPrecision***
[`double`](../../data-types.md) | Rounding precision ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"catalogGroupId":14,"price":1500,"roundType":2,"roundPrecision":10}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.roundingRule.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":1,"fields":{"catalogGroupId":14,"price":1500,"roundType":2,"roundPrecision":10},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.roundingRule.update
    ```

- JS

    ```js
    BX24.callMethod(
        'catalog.roundingRule.update', 
        {
            id: 1,
            fields: {
                catalogGroupId: 14,
                price: 1500,
                roundType: 2,
                roundPrecision: 10,
            }
        },
        function(result)
        {
            if(result.error())
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
        'catalog.roundingRule.update',
        [
            'id' => 1,
            'fields' => [
                'catalogGroupId' => 14,
                'price' => 1500,
                'roundType' => 2,
                'roundPrecision' => 10
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
        "roundingRule": {
            "catalogGroupId": 14,
            "createdBy": 1,
            "dateCreate": "2024-09-26T15:09:58+02:00",
            "dateModify": "2024-09-26T15:10:37+02:00",
            "id": 1,
            "modifiedBy": 1,
            "price": 1500,
            "roundPrecision": 10,
            "roundType": 2
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-09-26T15:09:58+02:00",
        "date_finish": "2024-09-26T15:09:58+02:00",
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
|| **roundingRule**
[`catalog_rounding_rule`](../data-types.md#catalog_rounding_rule) | Object with information about the updated price rounding rule ||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: catalogGroupId"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `200900000000` | Price rounding rule with this identifier does not exist
||
|| `100` | Parameter `id` not specified
||
|| `100` | Parameter `fields` not specified or empty
||
|| `0` | Required fields of the `fields` structure not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./catalog-rounding-rule-add.md)
- [{#T}](./catalog-rounding-rule-get.md)
- [{#T}](./catalog-rounding-rule-list.md)
- [{#T}](./catalog-rounding-rule-delete.md)
- [{#T}](./catalog-rounding-rule-get-fields.md)