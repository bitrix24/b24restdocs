# Get the Complete Set of Field Visibility Settings rpa.fields.getSettings

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Use [Smart processes](../../../crm/universal/user-defined-object-types/index.md) as an alternative functionality.

{% endnote %}

This method retrieves the complete set of field visibility settings for the stage with the identifier `stageId` in the process with the identifier `typeId`.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId***  
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **stageId**  
[`integer`](../../../data-types.md) | Identifier of the stage.

Defaults to `0`, which means general settings ||
|#

## Response Handling

HTTP Status: **200**

```json
{
    "fields": {
        "kanban": {
            "id": true,
            "UF_RPA_1_NAME": true
        },
        "create": {
            "UF_RPA_1_NAME": true
        }
    }
}
```

## Continue Your Exploration 

- [{#T}](./index.md)
- [{#T}](./rpa-fields-set-settings.md)
- [{#T}](./rpa-fields-set-visibility-settings.md)