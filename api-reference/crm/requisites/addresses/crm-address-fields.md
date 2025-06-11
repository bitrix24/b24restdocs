# Get Description of Address Fields crm.address.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a formal description of the address fields.

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.address.fields
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.address.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.address.fields",
        {},
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.address.fields',
        []
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
        "TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "TYPE_ID"
        },
        "ENTITY_TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ENTITY_TYPE_ID"
        },
        "ENTITY_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ENTITY_ID"
        },
        "ADDRESS_1": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Street, house, building, structure"
        },
        "ADDRESS_2": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Apartment / office"
        },
        "CITY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "City"
        },
        "POSTAL_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Postal code"
        },
        "REGION": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Region"
        },
        "PROVINCE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Province"
        },
        "COUNTRY": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Country"
        },
        "COUNTRY_CODE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "COUNTRY_CODE"
        },
        "LOC_ADDR_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Location address identifier"
        },
        "ANCHOR_TYPE_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ANCHOR_TYPE_ID"
        },
        "ANCHOR_ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ANCHOR_ID"
        }
    },
    "time": {
        "start": 1712938174.436428,
        "finish": 1712938175.432068,
        "duration": 0.9956400394439697,
        "processing": 0.5710320472717285,
        "date_start": "2024-04-12T19:09:34+02:00",
        "date_finish": "2024-04-12T19:09:35+02:00"
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`object`](../../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the field identifier and `value` is the object with [field attributes](#attributes) ||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time ||
|#

### Address Field Descriptions

#|
|| **Name**
`type` | **Description** ||
|| **TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the address type. Enumeration element "Address Type".

Enumeration elements for "Address Type" can be obtained using the method [crm.enum.addresstype](../../auxiliary/enum/crm-enum-address-type.md) 
||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object type.

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

Addresses can only be linked to Requisites (and requisites to companies or contacts) or Leads.

For backward compatibility, the ability to link Addresses to Contacts or Companies is retained. However, this link is only possible on some older accounts where the old address handling mode was specifically enabled by support.
||
|| **ENTITY_ID**
[`string`](../../../data-types.md) | Identifier of the parent object ||
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

This field contains the identifier of the address object in the `Location` module, associated with the CRM address object. Each CRM address corresponds to an address object in the `location` module. This can be used to copy an existing address into CRM with location information that is not present in the CRM address fields.

If the identifier of the `location` module address is specified when creating an address, a copy of the `location` address is created and linked to the created CRM address. If no values are specified for the string address fields in this case, they will be filled from the location address.

If at least one string field is specified, only the specified fields will be saved in the CRM address, and their values will overwrite the corresponding values in the location address object. The same behavior will occur when updating the address.
||
|| **ANCHOR_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the main parent object type.

This field is for internal use. The value is automatically filled when adding an address.

Object type identifiers can be obtained using the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md).

This field contains the identifier of the parent object type of the requisite (company or contact) if the address is linked to the requisite. If the address is linked to a lead, this value will be the lead type identifier.
||
|| **ANCHOR_ID**
[`integer`](../../../data-types.md) | This field is for internal use. The value is automatically filled when adding an address.

This field contains the identifier of the parent object of the requisite (company or contact) if the address is linked to the requisite. If the address is linked to a lead, this value will be the lead identifier.
||
|#

### Attribute Descriptions {#attributes}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Field type ||
|| **isRequired**
[`boolean`](../../../data-types.md) | "Required" attribute. Possible values:
- true — yes
- false — no
||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | "Read-only" attribute. Possible values:
- true — yes
- false — no
||
|| **isImmutable**
[`boolean`](../../../data-types.md) | "Immutable" attribute. Possible values:
- true — yes
- false — no
||
|| **isMultiple**
 [`boolean`](../../../data-types.md) | "Multiple" attribute. Possible values:
- true — yes
- false — no
||
|| **isDynamic**
[`boolean`](../../../data-types.md) | "Custom" attribute. Possible values:
- true — yes
- false — no
||
|| **title**
[`string`](../../../data-types.md) | Field identifier ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-address-add.md)
- [{#T}](./crm-address-update.md)
- [{#T}](./crm-address-list.md)
- [{#T}](./crm-address-delete.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-company-with-requisite.md)
- [{#T}](../../../../tutorials/crm/how-to-add-crm-objects/how-to-add-contact-with-requisite.md)