# Get Description of Custom Fields for the CRM Requisite Template crm.requisite.preset.field.fields

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method returns a formal description of the fields that define the customizable field of the requisite template.

No parameters.

## Code Examples

{% include [Examples Note](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.fields
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.fields
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.fields",
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
        'crm.requisite.preset.field.fields',
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
        "FIELD_NAME": {
            "type": "string",
            "isRequired": true,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Name"
        },
        "FIELD_TITLE": {
            "type": "string",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Template Title"
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
        "IN_SHORT_LIST": {
            "type": "char",
            "isRequired": false,
            "isReadOnly": false,
            "isImmutable": false,
            "isMultiple": false,
            "isDynamic": false,
            "title": "Show in Short List"
        }
    },
    "time": {
        "start": 1716823643.597269,
        "finish": 1716823643.949143,
        "duration": 0.35187387466430664,
        "processing": 0.012942075729370117,
        "date_start": "2024-05-27T17:27:23+02:00",
        "date_finish": "2024-05-27T17:27:23+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Title**
`type` | **Description** ||
|| **result**
[`object`](../../../../data-types.md) | An object in the format `{"field_1": "value_1", ... "field_N": "value_N"}`, where `field` is the field identifier and `value` is the object with [field attributes](#attribute) ||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
|#

### Description of Fields Defining the Customizable Field of the Requisite Template

#|
||  **Title**
`type` | **Description** ||
|| **ID**
[`integer`](../../../../data-types.md) | Field identifier. Created automatically and unique within the template ||
|| **FIELD_NAME**
[`string`](../../../../data-types.md) | Field name ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | Alternative name for the field in the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used 
||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. Order in the list of template fields ||
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N` ||
|#

### Description of Attributes {#attribute}

#|
|| **Title**
`type` | **Description** ||
|| **type**
[`string`](../../../../data-types.md) | Field type ||
|| **isRequired**
[`boolean`](../../../../data-types.md) | "Required" attribute. Possible values:
- true — yes
- false — no
||
|| **isReadOnly**
[`boolean`](../../../../data-types.md) | "Read-only" attribute. Possible values:
- true — yes
- false — no 
||
|| **isImmutable**
[`boolean`](../../../../data-types.md) | "Immutable" attribute. Possible values:
- true — yes
- false — no 
||
|| **isMultiple**
[`boolean`](../../../../data-types.md) | "Multiple" attribute. Possible values:
- true — yes
- false — no
||
|| **isDynamic**
[`boolean`](../../../../data-types.md) | "Custom" attribute. Possible values:
- true — yes
- false — no 
||
|| **title**
[`string`](../../../../data-types.md) | Field identifier ||
|#

## Error Handling

{% include [system errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)