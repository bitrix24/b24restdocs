# Set Full Visibility Settings for Fields rpa.fields.setSettings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- missing fields table
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.fields.setSettings` sets the full visibility settings for fields at the stage with the identifier stageId of the process with the identifier typeId.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId**^*^ 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **stageId** 
[`number`](../../../data-types.md) | Identifier of the stage. Default is 0 (general settings). ||
|| **fields**^*^ 
[`array`](../../../data-types.md) | Array with visibility settings for fields. If an empty fields array is passed, all settings will be cleared. ||
|#

{% include [Footnote on parameters](../../../../_includes/required.md) %}

## Example

```json
{
    "typeId": 1,
    "fields": {
        "kanban": [
            "createdBy", 
            "UF_RPA_1_NAME"
        ]
    }
}
```
The method will return a result similar to the request `rpa.fields.getSettings`.

{% include [Footnote on examples](../../../../_includes/examples.md) %}