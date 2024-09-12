# Delete Custom Field from CRM Requisite Template crm.requisite.preset.field.delete

> Scope: [`crm`](../../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method removes a custom field from the requisite template.

## Method Parameters

{% include [Note on Required Parameters](../../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **id***
[`integer`](../../../../data-types.md) | Identifier of the custom field to be deleted.

Identifiers of custom fields in the requisite template can be obtained using the [crm.requisite.preset.field.list](./crm-requisite-preset-field-list.md) method. ||
|| **preset***
[`object`](../../../../data-types.md) | An object containing the identifier of the template from which the custom field is being removed (e.g., `{"ID": 27}`).

Template identifiers can be obtained using the [crm.requisite.preset.list](../crm-requisite-preset-list.md) method. ||
|#

## Code Examples

{% include [Note on Examples](../../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":27,"preset":{"ID":1}}' \
    https://**put_your_bitrix24_address**/rest/**put_your_user_id_here**/**put_your_webhook_here**/crm.requisite.preset.field.delete
    ```

- cURL (OAuth) 

    ```bash
    curl -X POST \
    -H "Content-Type: application/json" \
    -H "Accept: application/json" \
    -d '{"ID":27,"preset":{"ID":1},"auth":"**put_access_token_here**"}' \
    https://**put_your_bitrix24_address**/rest/crm.requisite.preset.field.delete
    ```

- JS

    ```js
    BX24.callMethod(
        "crm.requisite.preset.field.delete",
        {
            ID: 27,        // Identifier of the custom field to be deleted from the template
            preset:
            {
                "ID": 1    // Identifier of the requisite template
            }
        },
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
        'crm.requisite.preset.field.delete',
        [
            'ID' => 27,
            'preset' => ['ID' => 1]
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
        "start": 1716819592.314096,
        "finish": 1716819592.798008,
        "duration": 0.48391199111938477,
        "processing": 0.06737184524536133,
        "date_start": "2024-05-27T16:19:52+02:00",
        "date_finish": "2024-05-27T16:19:52+02:00",
        "operating": 0
    }
}
```

### Returned Data

#|
|| **Name**
`type` | **Description** ||
|| **result**
[`boolean`](../../../../data-types.md) | Result of removing the custom field from the template:
- `true` — deleted
- `false` — not deleted 
||
|| **time**
[`time`](../../../../data-types.md) | Information about the request execution time ||
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
|| `The PresetField with ID '27' is not found` | The field with the specified identifier was not found. ||
|| `The Preset with ID '1' is not found` | The template with the specified identifier was not found. ||
|| `ID is not defined or invalid` | The template identifier is not specified or has an invalid value. ||
|| `Access denied` | Insufficient access permissions to delete the field from the requisite template. ||
|#

{% include [System Errors](../../../../../_includes/system-errors.md) %}

## Continue Learning

- [{#T}](./crm-requisite-preset-field-add.md)
- [{#T}](./crm-requisite-preset-field-update.md)
- [{#T}](./crm-requisite-preset-field-available-to-add.md)
- [{#T}](./crm-requisite-preset-field-get.md)
- [{#T}](./crm-requisite-preset-field-list.md)
- [{#T}](./crm-requisite-preset-field-fields.md)