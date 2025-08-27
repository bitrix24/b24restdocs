# Delete VAT Rate crm.vat.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: user with CRM administrator rights

{% note warning "Method development halted" %}

The method `crm.vat.delete` continues to function, but there is a more relevant alternative [catalog.vat.delete](../../../catalog/vat/catalog-vat-delete.md).

{% endnote %}

The method `crm.vat.delete` removes a VAT rate by its identifier.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../../data-types.md) | Identifier of the VAT rate to be deleted. 
You can obtain a list of rates with identifiers using the method [crm.vat.list](./crm-vat-list.md) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"id":7}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.vat.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":7,"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.vat.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.vat.delete",
    		{
    			id: 7
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.dir(result);
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
                'crm.vat.delete',
                [
                    'id' => 7
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
        echo 'Error deleting VAT: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.vat.delete",
        {
            id: 7
        },
        function(result) {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.vat.delete',
        [
            'id' => 7
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
        "start": 1751983429.678159,
        "finish": 1751983429.873604,
        "duration": 0.19544506072998047,
        "processing": 0.01796698570251465,
        "date_start": "2025-07-08T17:03:49+02:00",
        "date_finish": "2025-07-08T17:03:49+02:00",
        "operating_reset_at": 1751984029,
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
    "error": "Invalid identifier.",
    "error_description": "An invalid identifier was provided."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `The Commercial Catalog module is not installed.` | The catalog module is not installed ||
|| `400`     | `Invalid identifier.` | An invalid identifier was provided ||
|| `400`     | `Access denied.` | No rights to perform the operation ||
|| `400`     | `Error on deleting VAT rate.` | Error while deleting the VAT rate ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-vat-fields.md)
- [{#T}](./crm-vat-list.md)
- [{#T}](./crm-vat-get.md)
- [{#T}](./crm-vat-add.md)
- [{#T}](./crm-vat-update.md)