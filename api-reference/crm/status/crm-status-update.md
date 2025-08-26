# Update CRM Status Element `crm.status.update`

> Scope: [`crm`](../../scopes/permissions.md)
>
> Who can execute the method: user with CRM administrator rights

The method `crm.status.update` updates the parameters of an existing CRM status element.

## Method Parameters

{% include [Note on parameters](../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id*** 
[`integer`](../../data-types.md) | Identifier of the status element to be updated. You can get the list using the method [crm.status.list](./crm-status-list.md) ||
|| **fields*** 
[`object`](../../data-types.md) | Array of fields to update. The list of available fields is described [below](#fields)  ||
|#

### Fields Parameter {#fields}

#|
|| **Name**
 `type` | **Description** ||
|| **NAME**
[`string`](../../data-types.md) | Name ||
|| **SORT**
[`integer`](../../data-types.md) | Sorting ||
|| **COLOR**
[`string`](../../data-types.md) | Hex color code, for example `#39A8EF` ||
|#

## Code Examples

{% include [Note on examples](../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
         -H "Content-Type: application/json" \
         -H "Accept: application/json" \
         -d '{"id":123,"fields":{"NAME":"New Name","COLOR":"#00A9F4"}}' \
         https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.status.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"id":123,"fields":{"NAME":"New Name","COLOR":"#00A9F4"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.status.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		'crm.status.update',
    		{
    			id: 123,
    			fields: {
    				NAME: 'New Name',
    				COLOR: '#00A9F4'
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if (result.error())
    	{
    		console.error(result.error());
    	}
    	else
    	{
    		console.dir(result);
    	}
    }
    catch( error )
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
                'crm.status.update',
                [
                    'id' => 123,
                    'fields' => [
                        'NAME' => 'New Name',
                        'COLOR' => '#00A9F4'
                    ]
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
        if ($result->error()) {
            error_log($result->error());
            echo 'Error updating status: ' . $result->error();
        } else {
            echo 'Success: ' . print_r($result->data(), true);
        }
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating status: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.status.update",
        {
            id: 123,
            fields: {
                NAME: "New Name",
                COLOR: "#00A9F4"
            }
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
        'crm.status.update',
        [
            'id' => 123,
            'fields' => [
                'NAME' => 'New Name',
                'COLOR' => '#00A9F4'
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
    "result": true,
    "time": {
        "start": 1752149050.805837,
        "finish": 1752149050.842422,
        "duration": 0.036585092544555664,
        "processing": 0.009345054626464844,
        "date_start": "2025-07-10T15:04:10+02:00",
        "date_finish": "2025-07-10T15:04:10+02:00",
        "operating_reset_at": 1752149650,
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../data-types.md) | Root element of the response, contains `true` in case of success ||
|| **time**
[`time`](../../data-types.md#time) | Information about the execution time of the request ||
|#

## Error Handling

HTTP Status: **400**

```json
{
    "error": "Invalid identifier.",
    "error_description": "An invalid identifier was provided."
}
```

{% include notitle [error handling](../../../_includes/error-info.md) %}

### Possible Error Codes

#|
|| **Code** | **Description** | **Value** ||
|| `400`     | `Access denied.` | No rights to perform the operation ||
|| `400`     | `Invalid identifier.` | An invalid identifier was provided ||
|| `400`     | `Status is not found.` | Element not found ||
|| `400`     | `Error on updating status.` | Error while updating the element ||
|#

{% include [system errors](../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-status-fields.md)
- [{#T}](./crm-status-list.md)
- [{#T}](./crm-status-get.md)
- [{#T}](./crm-status-add.md)
- [{#T}](./crm-status-delete.md) 
- [{#T}](../../../tutorials/crm/how-to-add-crm-objects/how-to-add-category-to-spa.md)