# Update Address for crm.address.update

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the address for a contact or lead.

## Method Parameters

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` to update the address.

Values for the fields **TYPE_ID**, **ENTITY_TYPE_ID**, **ENTITY_ID** must be specified as they identify the address being modified. Other fields are specified if their values need to be changed ||
|#

### Parameter fields

{% include [Note on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier for the address type. Enumeration element "Address Type".

Enumeration elements for "Address Type" can be retrieved using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md)
||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier for the parent object type.

Object type identifiers can be retrieved using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Contacts (which are linked to companies or contacts) or Leads.

For backward compatibility, the ability to link Addresses to Contacts or Companies has been retained. However, this linkage is only possible on some older accounts where the old address handling mode was specifically enabled by support.
||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Identifier for the parent object ||
|| **ADDRESS_1**
[`string`](../../../data-types.md) | Street, house, building, structure ||
|| **ADDRESS_2**
[`string`](../../../data-types.md) | Apartment / office ||
|| **CITY**
[`string`](../../../data-types.md) | City ||
|| **POSTAL_CODE**
[`string`](../../../data-types.md) | Postal code ||
|| **REGION**
[`string`](../../../data-types.md) | District ||
|| **PROVINCE**
[`string`](../../../data-types.md) | State ||
|| **COUNTRY**
[`string`](../../../data-types.md) | Country ||
|| **COUNTRY_CODE**
[`string`](../../../data-types.md) | Country code.

Not used, retained for backward compatibility. An empty string can be specified as a value.
||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) | 
Identifier for the location address.

This field contains the identifier of the address object in the `Location` module, linked to the CRM address object. Each CRM address corresponds to an address object in the `location` module. This can be used to copy an existing address into CRM with location information that is not present in the CRM address fields.

If the identifier of the `location` module address is specified when creating an address, a copy of the `location` address is created and linked to the newly created CRM address. If no values are specified for the string address fields in this case, they will be filled from the location address.

If at least one string field is specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior will occur when updating the address.
||
|#

## Code Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":3,"ENTITY_ID":1,"ADDRESS_1":"Moscow Avenue, 261","CITY":"Kaliningrad"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.update
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":3,"ENTITY_ID":1,"ADDRESS_1":"Moscow Avenue, 261","CITY":"Kaliningrad"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.update
    ```

- JS

    ```js
    try
    {
    	const response = await $b24.callMethod(
    		"crm.address.update",
    		{
    			fields:
    			{
    				"TYPE_ID": 1,           //
    				"ENTITY_TYPE_ID": 3,    // - Identifying fields.
    				"ENTITY_ID": 1,         //
    				"ADDRESS_1": "Moscow Avenue, 261", // - Fields whose values are changing.
    				"CITY": "Kaliningrad"                    //
    			}
    		}
    	);
    	
    	const result = response.getData().result;
    	if(result.error())
    	{
    		console.error(result.error());
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
                'crm.address.update',
                [
                    'fields' => [
                        'TYPE_ID'        => 1,
                        'ENTITY_TYPE_ID' => 3,
                        'ENTITY_ID'      => 1,
                        'ADDRESS_1'      => 'Moscow Avenue, 261',
                        'CITY'           => 'Kaliningrad',
                    ],
                ]
            );
    
        $result = $response
            ->getResponseData()
            ->getResult();
    
    } catch (Throwable $e) {
        error_log($e->getMessage());
        echo 'Error updating address: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.address.update",
        {
            fields:
            {
                "TYPE_ID": 1,           //
                "ENTITY_TYPE_ID": 3,    // - Identifying fields.
                "ENTITY_ID": 1,         //
                "ADDRESS_1": "Moscow Avenue, 261", // - Fields whose values are changing.
                "CITY": "Kaliningrad"                    //
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
        }
    );
    ```

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.address.update',
        [
            'fields' => [
                'TYPE_ID' => 1,
                'ENTITY_TYPE_ID' => 3,
                'ENTITY_ID' => 1,
                'ADDRESS_1' => 'Moscow Avenue, 261',
                'CITY' => 'Kaliningrad'
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
[`boolean`](../../../data-types.md) | Result of the address update:
- `true` — updated
- `false` — not updated 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "ENTITY_ID is not defined or invalid."
}
```

{% include notitle [error handling](../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `TYPE_ID is not defined or invalid` | Address type identifier is not specified or has an invalid value ||
|| `ENTITY_TYPE_ID is not defined or invalid` | Parent object type identifier is not specified or has an invalid value. ||
|| `ENTITY_ID is not defined or invalid` | Parent object identifier is not specified or has an invalid value. ||
|| `TypeAddress not found` | Address not found ||
|| `Access denied` | Insufficient access permissions to update the address ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-add.md)
- [{#T}](./crm-address-list.md)
- [{#T}](./crm-address-delete.md)
- [{#T}](./crm-address-fields.md)