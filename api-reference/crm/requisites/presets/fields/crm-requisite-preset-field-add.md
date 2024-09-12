# Add a Custom Field to the CRM Requisite Template crm.requisite.preset.field.add

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method adds a custom field to the requisite template. You can use the method [crm.requisite.preset.field.availabletoadd](./crm-requisite-preset-field-available-to-add.md) to retrieve fields available for addition to the template.

Before adding a user-defined field `UF_...` to the template, it must be created using the method [crm.requisite.userfield.add](../../user-fields/crm-requisite-userfield-add.md) or you need to ensure that it already exists.

## Method Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template to which the custom field is being added (e.g., `{"ID": 27}`).

Template identifiers can be obtained using the method [crm.requisite.preset.list](../crm-requisite-preset-list.md) ||
|| **fields***
[`object`](../../../../data-types.md) | A set of fields — an object of the form `{"field": "value"[, ...]}` for adding the custom field to the template ||
|#

### fields Parameter

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
||  **Name**
`type` | **Description** ||
|| **FIELD_NAME***
[`string`](../../../../data-types.md) | The name of the field. ||
|| **FIELD_TITLE**
[`string`](../../../../data-types.md) | An alternative name for the field in the requisite.

The alternative name is displayed in various forms for filling out requisites. Depending on the specific form, the alternative name may or may not be used. ||
|| **SORT**
[`integer`](../../../../data-types.md) | Sorting. The order in the list of template fields. ||
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
    -d '{"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"TEST","IN_SHORT_LIST":"N","SORT":580}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.add
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"preset":{"ID":27},"fields":{"FIELD_NAME":"RQ_NAME","FIELD_TITLE":"TEST","IN_SHORT_LIST":"N","SORT":580},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.add
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.add",
        {
            preset:
            {
                "ID": 27    // Template identifier
            },
            fields:        // Object describing the custom field
            {
                "FIELD_NAME": "RQ_NAME",
                "FIELD_TITLE": "TEST",
                "IN_SHORT_LIST": "N",
                "SORT": 580
            }
        },
        function(result)
        {
            if(result.error())
                console.error(result.error());
            else
                console.info("Custom field added to the template with ID " + result.data());
        }
    );
    ```

- PHP

    ```php
    require_once('crest.php');

    $result = CRest::call(
        'crm.requisite.preset.field.add',
        [
            'preset' => ['ID' => 27],
            'fields' => [
                'FIELD_NAME' => 'RQ_NAME',
                'FIELD_TITLE' => 'TEST',
                'IN_SHORT_LIST' => 'N',
                'SORT' => 580
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
    "result": 3,
    "time": {
        "start": 1716811635.108203,
        "finish": 1716811635.503093,
        "duration": 0.39489006996154785,
        "processing": 0.043524980545043945,
        "date_start": "2024-05-27T14:07:15+02:00",
        "date_finish": "2024-05-27T14:07:15+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`integer`](../../../../data-types.md) | The identifier of the added field. ||
|| **time**
[`time`](../../../../data-types.md) | Information about the execution time of the request. ||
|#

## Error Handling

HTTP Status: **40x**, **50x**

```json
{
    "error": "",
    "error_description": "The field 'RQ_NAME' cannot be added."
}
```

{% include notitle [Error Handling](../../../../../_includes/error-info.md) %}

### Possible Error Codes

#|  
|| **Code** | **Description** ||
|| `The field 'RQ_NAME' cannot be added` | The field cannot be added. The field may already exist in the template or it is not available for the country to which the template belongs. ||
|| `The Preset with ID '27' is not found` | The template with the specified identifier was not found. ||
|| `Access denied` | Insufficient access permissions to add the custom field to the requisite template. ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-delete.md)
- [{#T}](./crm-requisite-preset-field-fields.md)