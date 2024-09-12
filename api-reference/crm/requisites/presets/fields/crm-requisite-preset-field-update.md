# Update Custom Field of a Given CRM Requisite Template

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates a custom field in the requisite template.

## Method Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the custom field to be updated. 

Identifiers of custom fields in the requisite template can be obtained using the [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) method. ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template to which the custom field is added (e.g., `{"ID": 27}`). 

Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method. ||
|| **fields***
[`object`](../../../../data-types.md) | An object with fields and their values that need to be updated. The [crm.requisite.preset.field.fields](./crm-requisite-preset-field-fields.md) method allows you to get a description of the fields that can be modified. 

The API requires a value to be specified in the **FIELD_NAME** field. If it does not need to be changed, the current value can be specified. ||
|#

### Fields Parameter

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
||  **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../../../data-types.md) | Name of the field. 

The API requires a value to be specified in this field. If it does not need to be changed, the current value can be specified. ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | Alternative name for the requisite field.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used. ||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. Order in the list of template fields. ||
|| **IN_SHORT_LIST**
[`char`](../../../../data-types.md) | Show in the short list. Deprecated field, currently not used. Retained for backward compatibility. Can take values `Y` or `N`. ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"Name","IN_SHORT_LIST":"Y"}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.update
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":1,"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"Name","IN_SHORT_LIST":"Y"},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.update
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.update",
        {
            ID: 1,          // Identifier of the custom field to be updated
            preset:
            {
                "ID": 27    // Identifier of the requisite template
            },
            fields:         // Values of the fields to be updated
            {
                "FIELD_NAME": "RQ_NAME",    // The API requires a value to be specified in this field. If
                                            // the value does not need to be changed, keep the current one.
                "FIELD_TITLE": "Name",
                "IN_SHORT_LIST": "Y",
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
            {
                console.info(result.data());
            }
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.field.update',
        [
            'ID' => 1,
            'preset' => ['ID' => 27],
            'fields' => [
                'FIELD_NAME' => 'RQ_NAME',
                'FIELD_TITLE' => 'Name',
                'IN_SHORT_LIST' => 'Y',
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
        "start": 1716898310.398361,
        "finish": 1716898310.936332,
        "duration": 0.537971019744873,
        "processing": 0.09376883506774902,
        "date_start": "2024-05-28T14:11:50+02:00",
        "date_finish": "2024-05-28T14:11:50+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../data-types.md) | Result of updating the custom field:
- `true` — updated
- `false` — not updated 
||
|| **time**
[`time`](../../../data-types.md) | Information about the request execution time. ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The PresetField with ID '27' is not found"
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `The PresetField with ID '1' is not found` | The field with the specified identifier was not found. ||
|| `The Preset with ID '27' is not found` | The template with the specified identifier was not found. ||
|| `ID is not defined or invalid` | The field identifier is not specified or has an invalid value. ||
|| `Access denied` | Insufficient access permissions to delete the field from the requisite template. ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)