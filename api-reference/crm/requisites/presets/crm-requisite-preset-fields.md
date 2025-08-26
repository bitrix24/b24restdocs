# Get Description of the Fields of the Requisite Template crm.requisite.preset.fields

> Scope: [`crm`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method returns a formal description of the fields of the requisite template.

No parameters.

## Code Examples

{% include [Examples Note](../../../../_includes/examples.md) %}

Searching for templates by country binding:

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.fields
    ```

- cURL (OAuth)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.fields
    ```

- JS

    ```js
    try
    {
        const response = await $b24.callMethod(
            "crm.requisite.preset.fields",
            {}
        );
        
        const result = response.getData().result;
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.dir(result);
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
                'crm.requisite.preset.fields',
                []
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
        echo 'Error fetching requisite preset fields: ' . $e->getMessage();
    }
    ```

- BX24.js

    ```js
    BX24.callMethod(
        "crm.requisite.preset.fields",
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

- PHP CRest

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.fields',
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
        "ID": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "ID"
        },
        "ENTITY_TYPE_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Object Type ID"
        },
        "COUNTRY_ID": {
            "type": "integer",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": true,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Country ID"
        },
        "NAME": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Name"
        },
        "DATE_CREATE": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Creation Date"
        },
        "DATE_MODIFY": {
            "type": "datetime",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Modification Date"
        },
        "CREATED_BY_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Created By"
        },
        "MODIFY_BY_ID": {
            "type": "user",
            "isRequired": false,
            "isReadOnly": true,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Modified By"
        },
        "ACTIVE": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Active"
        },
        "SORT": {
            "type": "integer",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Sorting"
        },
        "XML_ID": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "External Code"
        }
    },
    "time": {
        "start": 1716555914.663808,
        "finish": 1716555914.996533,
        "duration": 0.33272480964660645,
        "processing": 0.008843183517456055,
        "date_start": "2024-05-24T15:05:14+02:00",
        "date_finish": "2024-05-24T15:05:14+02:00",
        "operating": 0
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

### Description of the Requisite Template Fields

#|
|| **Name**
`type` | **Description** ||
|| **ID**
[`integer`](../../../data-types.md) | Identifier of the requisite. Created automatically and unique within the account ||
|| **ENTITY_TYPE_ID**
[`integer`](../../../data-types.md) | Identifier of the parent object's type. Currently, this is only "Requisite" (identifier `8`).

The identifiers of CRM object types are provided by the method [crm.enum.ownertype](../../auxiliary/enum/crm-enum-owner-type.md) 
||
|| **COUNTRY_ID**
[`integer`](../../../data-types.md) | Identifier of the country corresponding to the set of fields of the requisite template (to get available values, see the method [crm.requisite.preset.countries](./crm-requisite-preset-countries.md)) ||
|| **DATE_CREATE**
[`datetime`](../../../data-types.md) | Creation date ||
|| **DATE_MODIFY**
[`datetime`](../../../data-types.md) | Modification date. Contains an empty string if the template has not been changed since creation ||
|| **CREATED_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who created the requisite ||
|| **MODIFY_BY_ID**
[`user`](../../../data-types.md) | Identifier of the user who modified the requisite ||
|| **NAME**
[`string`](../../../data-types.md) | Name of the requisite ||
|| **XML_ID**
[`string`](../../../data-types.md) | External key. Used for exchange operations. Identifier of the external information base object. 

The purpose of the field may change by the final developer. 

Each application ensures the uniqueness of values in this field. It is recommended to use a unique prefix to avoid collisions with other applications. 

In CRM, values of the form `#CRM_REQUISITE_PRESET_DEF_...` are reserved for identifying templates that are used by default. These identifiers should not be used for your purposes, as this may lead to logic violations ||
|| **ACTIVE**
[`char`](../../../data-types.md) | Activity status. Uses values `Y` or `N`. Determines the availability of the template in the selection list when adding requisites ||
|| **SORT**
[`integer`](../../../data-types.md) | Sorting ||
|#

### Description of Attributes {#attributes}

#|
|| **Name**
`type` | **Description** ||
|| **type**
[`string`](../../../data-types.md) | Field type ||
|| **isRequired**
[`boolean`](../../../data-types.md) | Required attribute. Possible values:
- true — yes
- false — no
||
|| **isReadOnly**
[`boolean`](../../../data-types.md) | Read-only attribute. Possible values:
- true — yes
- false — no
||
|| **isImmutable**
[`boolean`](../../../data-types.md) | Immutable attribute. Possible values:
- true — yes
- false — no
||
|| **isMultiple**
[`boolean`](../../../data-types.md) | Multi-field attribute. Possible values:
- true — yes
- false — no
||
|| **isDynamic**
[`boolean`](../../../data-types.md) | Custom attribute. Possible values:
- true — yes
- false — no
||
|| **title**
[`string`](../../../data-types.md) | Field identifier ||
|#

## Error Handling

{% include [system errors](../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-add.md)
- [{#T}](./crm-requisite-preset-update.md)
- [{#T}](./crm-requisite-preset-countries.md)
- [{#T}](./crm-requisite-preset-get.md)
- [{#T}](./crm-requisite-preset-list.md)
- [{#T}](./crm-requisite-preset-delete.md)