# Add Address crm.address.add

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a new address for a property or lead. For the user, this address appears as the address of a contact, company, or lead.

Multiple addresses of different [types](../../auxiliary/enum/crm-enum-address-type.md) can be created and linked to a single property.

## Method Parameters

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **fields***
[`object`](../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding the address ||
|#

### Parameter fields

{% include [Footnote on required parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

Enumeration elements of "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) 
||
|| **ENTITY_TYPE_ID***
[`integer`](../../../data-types.md) | Identifier of the parent object type.

Identifiers of object types can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Properties (which are already linked to companies or contacts) or Leads.

For backward compatibility, the ability to link Addresses to Contacts or Companies has been retained. However, this linkage is only possible on some older accounts where the old address handling mode has been specifically enabled by support.
||
|| **ENTITY_ID***
[`string`](../../../data-types.md) | Identifier of the parent object ([property](../universal/index.md) or [lead](../../leads/index.md)) ||
|| **ADDRESS_1**
[`string`](../../../data-types.md) | Street, house, building, structure ||
|| **ADDRESS_2**
[`string`](../../../data-types.md) | Apartment / office ||
|| **CITY**
[`string`](../../../data-types.md) | City ||
|| **POSTAL_CODE**
[`string`](../../../data-types.md) | Postal code ||
|| **REGION**
[`string`](../../../data-types.md) | Region ||
|| **PROVINCE**
[`string`](../../../data-types.md) | Province ||
|| **COUNTRY**
[`string`](../../../data-types.md) | Country ||
|| **COUNTRY_CODE**
[`string`](../../../data-types.md) | Country code.

Not used, retained for backward compatibility. An empty string can be specified as a value.
||
|| **LOC_ADDR_ID**
[`integer`](../../../data-types.md) | Identifier of the location address.

This field contains the identifier of the address object in the `Location` module, linked to the CRM address object. Each CRM address corresponds to an address object in the `location` module. This can be used to copy an existing address into CRM with location information that is not present in the CRM address fields.

If the identifier of the `location` module address is specified when creating the address, a copy of the `location` address is created and linked to the newly created CRM address. If no values are specified for the string address fields in this case, they will be filled from the location address.

If at least one string field was specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior will occur when updating the address.
||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":8,"ENTITY_ID":1,"ADDRESS_1":"Mira Avenue, 4","ADDRESS_2":"Kaliningrad Regional Drama Theater","CITY":"Kaliningrad","POSTAL_CODE":"236036","REGION":"Kaliningrad Urban District","PROVINCE":"Kaliningrad Region","COUNTRY":"Russia"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.add
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"fields":{"TYPE_ID":1,"ENTITY_TYPE_ID":8,"ENTITY_ID":1,"ADDRESS_1":"Mira Avenue, 4","ADDRESS_2":"Kaliningrad Regional Drama Theater","CITY":"Kaliningrad","POSTAL_CODE":"236036","REGION":"Kaliningrad Urban District","PROVINCE":"Kaliningrad Region","COUNTRY":"Russia"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.address.add",
        {
            fields:
            {
                "TYPE_ID": 1,            // Address type, see crm.enum.addresstype
                "ENTITY_TYPE_ID": 8,     // Object type (property or lead)
                "ENTITY_ID": 1,          // Property identifier
                "ADDRESS_1": "Mira Avenue, 4",
                "ADDRESS_2": "Kaliningrad Regional Drama Theater",
                "CITY": "Kaliningrad",
                "POSTAL_CODE": "236036",
                "REGION": "Kaliningrad Urban District",
                "PROVINCE": "Kaliningrad Region",
                "COUNTRY": "Russia",
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.address.add',
        [
            'fields' => [
                'TYPE_ID' => 1,
                'ENTITY_TYPE_ID' => 8,
                'ENTITY_ID' => 1,
                'ADDRESS_1' => 'Mira Avenue, 4',
                'ADDRESS_2' => 'Kaliningrad Regional Drama Theater',
                'CITY' => 'Kaliningrad',
                'POSTAL_CODE' => '236036',
                'REGION' => 'Kaliningrad Urban District',
                'PROVINCE' => 'Kaliningrad Region',
                'COUNTRY' => 'Russia',
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
        "start": 1712922620.724857,
        "finish": 1712922623.393783,
        "duration": 2.6689260005950928,
        "processing": 2.210068941116333,
        "date_start": "2024-04-12T14:50:20+03:00",
        "date_finish": "2024-04-12T14:50:23+03:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of adding the address:
- `true` — added
- `false` — not added 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

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
|| `ENTITY_ID is not defined or invalid` | Parent object identifier is not specified or has an invalid value ||
|| `TypeAddress exists` | An address of this type already exists for the specified parent object ||
|| `Access denied` | Insufficient access permissions to add the address ||
|#

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-update.md)
- [{#T}](./crm-address-list.md)
- [{#T}](./crm-address-delete.md)
- [{#T}](./crm-address-fields.md)