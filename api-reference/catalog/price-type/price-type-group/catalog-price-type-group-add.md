# Add Price Type Binding to Customer Group catalog.priceTypeGroup.add

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: user with "Manage Price Types" access permission

The method `catalog.priceTypeGroup.add` adds a price type binding to a customer group.

## Method Parameters

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../data-types.md) | Field values for creating a price type binding to a customer group ([detailed description](#fields)) ||
|#

### Parameter fields {#fields}

{% include [Note on Required Parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId***
[`catalog_price_type.id`](../../data-types.md#catalog_price_type) | Identifier of the price type. Can be obtained using the [catalog.priceType.list](../catalog-price-type-list.md) method ||
|| **groupId***
[`integer`](../../data-types.md) | Identifier of the customer group ||
|| **access***
[`char`](../../data-types.md) | Type of access to the price. Possible values:
- `Y` — right to purchase at this price type
- `N` — right to view this price type ||
|#

{% note info "" %}

Before adding, check for an existing record using the [catalog.priceTypeGroup.list](./catalog-price-type-group-list.md) method with filters for `catalogGroupId`, `groupId`, and `access`. If the record already exists, the method will return an error: `The specified access type for this group already exists`

{% endnote %}


## Code Examples

{% include [Note on Examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":9,"groupId":23,"access":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeGroup.add
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"catalogGroupId":9,"groupId":23,"access":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeGroup.add
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.priceTypeGroup.add',
    		{
    			fields: {
    				catalogGroupId: 9,
    				groupId: 23,
    				access: "Y"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error.ex);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.priceTypeGroup.add',
                [
                    'fields' => [
                        'catalogGroupId' => 9,
                        'groupId'        => 23,
                        'access'         => 'Y',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error()->ex);
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error adding price type group: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceTypeGroup.add',
        {
            fields: {
                catalogGroupId: 9,
                groupId: 23,
                access: 'Y'
            }
        },
        function(result) {
            if (result.error())
                console.error(result.error().ex);
            else
                console.log(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.priceTypeGroup.add',
        [
            'fields' => [
                'catalogGroupId' => 9,
                'groupId' => 23,
                'access' => 'Y'
            ]
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
        "priceTypeGroup": {
        "access": "Y",
        "catalogGroupId": 9,
        "groupId": 23,
        "id": 109
        }
    },
    "time": {
        "start": 1774260171,
        "finish": 1774260171.438073,
        "duration": 0.43807291984558105,
        "processing": 0,
        "date_start": "2026-03-23T13:02:51+01:00",
        "date_finish": "2026-03-23T13:02:51+01:00",
        "operating_reset_at": 1774260771,
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
|| **priceTypeGroup**
[`catalog_price_type_group`](../../data-types.md#catalog_price_type_group) | Object containing information about the created price type binding to the customer group ||
|| **time**
[`time`](../../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": 200040300020,
    "error_description": "Access Denied"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `200040300020` | Access Denied | Insufficient rights to edit price types ||
|| `100` | Could not find value for parameter {fields} | Required parameter `fields` not provided ||
|| `0` | The specified price type does not exist | The specified price type does not exist ||
|| `0` | The specified group does not exist | The specified customer group does not exist ||
|| `0` | Invalid access type provided. The available values are: Y, N | Invalid access type provided. Available values: `Y`, `N` ||
|| `0` | The specified access type for this group already exists | This binding for this price type and group already exists ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-group-delete.md)
- [{#T}](./catalog-price-type-group-get-fields.md)
- [{#T}](./catalog-price-type-group-list.md)