# Delete payer type sale.persontype.delete

> Scope: [`sale`](../../scopes/permissions.md)
>
> Who can execute the method: administrator

This method deletes a payer type.

## Method Parameters

{% include [Note on required parameters](../../../_includes/required.md) %}

#|
|| **Parameter**
`type`| **Description** ||
|| **id***
[`sale_person_type.id`](../../data-types.md) | Identifier of the payer type ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/sale.persontype.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":5,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/sale.persontype.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'sale.persontype.delete',
    		{ id: 5 }
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.log(result);
    	}
    }
    catch(error)
    {
    	console.error('Error:', error);
    }
    ```

- PHP

    ```php
    try {
        $response = $b24Service
            ->core
            ->call(
                'sale.persontype.delete',
                [
                    'id' => 5,
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
        echo 'Error deleting person type: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        'sale.persontype.delete', 
        { id: 5 }, 
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
        'sale.persontype.delete',
        [
            'id' => 5
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
    "result": true,
    "time": {
        "start": 1712326002.94315,
        "finish": 1712326003.25198,
        "duration": 0.308833837509155,
        "processing": 0.0920200347900391,
        "date_start": "2024-04-05T16:06:42+02:00",
        "date_finish": "2024-04-05T16:06:43+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Result of deleting the payer type ||
|| **time**
[`time`](../data-types.md) | Information about the execution time of the request ||
|#

## Error Handling

HTTP status: **400**

```json
{
    "error": 200750000008,
    "error_description": "The payer type with ID=8 is used in orders",
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| `200750000003` | The payer type with the specified `id` has a protected field `code`, such a type cannot be deleted ||
|| `200040300020` | No access to editing ||
|| `200740400001` | The payer type with the specified `id` does not exist ||
|| `200750000008`
`200750000004` | Unable to delete the payer type.
 
Possible reasons:
- There is an order that uses the payer type with the specified `id`
- There is an archived order that uses the payer type with the specified `id`
 ||
|| `100` | The parameter `id` is not specified ||
|| `0` | Other errors (e.g., fatal errors) ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./sale-person-type-add.md)
- [{#T}](./sale-person-type-update.md)
- [{#T}](./sale-person-type-get.md)
- [{#T}](./sale-person-type-list.md)
- [{#T}](./sale-person-type-get-fields.md)