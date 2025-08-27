# Update VAT Rate catalog.vat.update

> Scope: [`catalog`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method updates the VAT rate.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`catalog_vat.id`](../data-types.md#catalog_vat) | Identifier of the VAT rate ||
|| **fields***
[`object`](../../data-types.md) | Field values for updating the VAT rate ([detailed description](#fields)) ||
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
||
|| **rate***
[`double`](../../data-types.md) | Value of the VAT rate ||
|| **sort**
[`integer`](../../data-types.md) | Sorting
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
    -d '{"id":6,"fields":{"name":"Tax 23%","rate":23,"sort":20,"active":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/catalog.vat.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":6,"fields":{"name":"Tax 23%","rate":23,"sort":20,"active":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/catalog.vat.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'catalog.vat.update', 
    		{
    			id: 6,
    			fields: {
    				name: "Tax 23%",
    				rate: 23,
    				sort: 20,
    				active: "Y"
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	console.log(result);
    }
    catch(error)
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
                'catalog.vat.update',
                [
                    'id' => 6,
                    'fields' => [
                        'name'   => "Tax 23%",
                        'rate'   => 23,
                        'sort'   => 20,
                        'active' => "Y"
                    ]
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
        echo 'Error updating VAT: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'catalog.vat.update', 
        {
            id: 6,
            fields: {
                name: "Tax 23%",
                rate: 23,
                sort: 20,
                active: "Y"
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
        'catalog.vat.update',
        [
            'id' => 6,
            'fields' => [
                'name' => 'Tax 23%',
                'rate' => 23,
                'sort' => 20,
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
            "id": 6,
            "name": "Tax 23%",
            "rate": 23,
            "sort": 20,
            "timestampX": "2024-09-16T11:53:04+02:00"
        }
    },
    "time": {
        "start": 1712327086.69665,
        "finish": 1712327086.95303,
        "duration": 0.256376028060913,
        "processing": 0.0112268924713135,
        "date_start": "2024-04-05T16:24:46+02:00",
        "date_finish": "2024-04-05T16:24:46+02:00",
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
[`catalog_vat`](../data-types.md#catalog_vat) | Object with information about the updated VAT rate
||
|| **time**
[`time`](../../data-types.md#time) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 0,
    "error_description":"Required fields: code"
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200040300020` | Insufficient permissions to edit
||
|| `200800000000` | No VAT rate exists with this identifier
||
|| `100` | Parameter `id` not specified
||
|| `100` | Parameter `fields` not specified or empty
||
|| `0` | Required fields in the `fields` structure not provided
|| 
|| `0` | Other errors (e.g., fatal errors)
|| 
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./catalog-vat-add.md)
- [{#T}](./catalog-vat-get.md)
- [{#T}](./catalog-vat-list.md)
- [{#T}](./catalog-vat-delete.md)
- [{#T}](./catalog-vat-get-fields.md)