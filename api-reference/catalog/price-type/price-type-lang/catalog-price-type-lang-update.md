# Update the translation of the price type name catalog.priceTypeLang.update

> Scope: [`catalog`](../../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the translation of the price type name by its identifier.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **Id***
[`catalog_price_type_lang.id`](../../data-types.md#catalog_price_type_lang) | Identifier of the price type name translation ||
|| **fields***
[`object`](../../../data-types.md) | Field values for updating the price type name translation ||
|#

### Fields Parameter

#|
|| **Name**
`type` | **Description** ||
|| **catalogGroupId**
[`catalog_price_type.id`](../../data-types.md#catalog_price_type) | Identifier of the price type.

You can obtain the identifiers of price types using the [catalog.priceType.list](../catalog-price-type-list.md) method.
||
|| **name**
[`string`](../../../data-types.md) | Translation of the price type name ||
|| **lang**
[`catalog_language.lid`](../../data-types.md#catalog_language) | Language identifier.

You can obtain the identifiers of languages using the [catalog.priceTypeLang.getLanguages](./catalog-price-type-lang-get-languages.md) method.
||
|#

At least one field must be specified in the `fields` parameter.

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"fields":{"name":"Base Price"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.priceTypeLang.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"fields":{"name":"Base Price"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.priceTypeLang.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.priceTypeLang.update', 
    		{
    			id: 6,
    			fields: {
    				name: "Base Price",
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch( error )
    {
    	console.error(error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'catalog.priceTypeLang.update',
                [
                    'id' => 6,
                    'fields' => [
                        'name' => "Base Price",
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating price type language: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.priceTypeLang.update', 
        {
            id: 6,
            fields: {
                name: "Base Price",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'catalog.priceTypeLang.update',
        [
            'id' => 6,
            'fields' => [
                'name' => 'Base Price'
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
        "priceTypeLang": {
            "catalogGroupId": 1,
            "id": 6,
            "lang": "kz",
            "name": "Base Price"
        }
    },
    "time": {
        "start": 1733843735.3651,
        "finish": 1733843735.75101,
        "duration": 0.385910987854004,
        "processing": 0.0111708641052246,
        "date_start": "2024-12-10T17:15:35+02:00",
        "date_finish": "2024-12-10T17:15:35+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | Root element of the response ||
|| **priceTypeLang**
[`catalog_price_type_lang`](../../data-types.md#catalog_price_type_lang) | Object containing information about the updated price type name translation ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: name"
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
|| 
|| `201200000000` | No translation of the price type name with such identifier exists
|| 
|| `201200000010` | Language with the specified identifier does not exist
|| 
|| `201000000000` | Price type with the specified identifier does not exist
|| 
|| `400` | Translation for the specified pair: price type and language identifier already exists
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

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-price-type-lang-add.md)
- [{#T}](./catalog-price-type-lang-get.md)
- [{#T}](./catalog-price-type-lang-list.md)
- [{#T}](./catalog-price-type-lang-delete.md)
- [{#T}](./catalog-price-type-lang-get-languages.md)
- [{#T}](./catalog-price-type-lang-get-fields.md)