# Delete address crm.address.delete

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method deletes an address.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for deleting the address ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

Enumeration elements for "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) 
||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object's type.

Identifiers for object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Details (which are already linked to companies or contacts) or Leads.

For backward compatibility, the ability to link Addresses to Contacts or Companies has been retained. However, this linkage is only possible on some older accounts where the old address handling mode was specifically enabled by technical support.
||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Identifier of the parent object ([detail](../universal/index.md) or [lead](../../leads/index.md)) ||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

Searching for addresses linked to the Detail type:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":3,"ENTITY_ID":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.delete
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":3,"ENTITY_ID":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.delete
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.address.delete",
    		{
    			fields:
    			{
    				"TYPE_ID": 1,
    				"ENTITY_TYPE_ID": 3,
    				"ENTITY_ID": 1
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    		console.error(result.error());
    	else
    		console.info(result);
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
                'crm.address.delete',
                [
                    'fields' => [
                        'TYPE_ID'       => 1,
                        'ENTITY_TYPE_ID' => 3,
                        'ENTITY_ID'     => 1,
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
        echo 'Error deleting address: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.address.delete",
        {
            fields:
            {
                "TYPE_ID": 1,
                "ENTITY_TYPE_ID": 3,
                "ENTITY_ID": 1
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info(result.data());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.address.delete',
        [
            'fields' => [
                'TYPE_ID' => 1,
                'ENTITY_TYPE_ID' => 3,
                'ENTITY_ID' => 1
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
    "result": true,
    "time": {
        "start": 1712922620.724857,
        "finish": 1712922623.393783,
        "duration": 2.6689260005950928,
        "processing": 2.210068941116333,
        "date_start": "2024-04-12T14:50:20+02:00",
        "date_finish": "2024-04-12T14:50:23+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of the address deletion:
- `true` — deleted
- `false` — not deleted
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "TypeAddress not found."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `TYPE_ID is not defined or invalid` | The address type identifier is not specified or has an invalid value ||
|| `ENTITY_TYPE_ID is not defined or invalid` | The parent object type identifier is not specified or has an invalid value ||
|| `ENTITY_ID is not defined or invalid` | The parent object identifier is not specified or has an invalid value ||
|| `TypeAddress not found` | The address to delete was not found ||
|| `Access denied` | Insufficient access permissions to delete the address ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-add.md)
- [{#T}](./crm-address-update.md)
- [{#T}](./crm-address-list.md)
- [{#T}](./crm-address-fields.md)