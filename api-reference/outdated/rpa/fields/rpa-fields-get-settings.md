# Get the complete set of field visibility settings rpa.fields.getSettings

{% note warning "We are still updating this page" %}

Some data may be missing — we will add it soon

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.fields.getSettings` will return the complete set of field visibility settings at the stage with the identifier stageId of the process with the identifier typeId.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId**^*^ 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **stageId** 
[`number`](../../../data-types.md) | Identifier of the stage. Default is 0 (general settings). ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Response on success

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