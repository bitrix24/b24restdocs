# Set Full Visibility Settings for rpa.fields.setSettings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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
[`number`](../../../data-types.md) | Process identifier. ||
|| **stageId** 
[`number`](../../../data-types.md) | Stage identifier. Default is 0 (general settings). ||
|| **fields**^*^ 
[`array`](../../../data-types.md) | Array with field visibility settings. If an empty fields is passed, all settings will be cleared. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

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

{% endlist %}

The method will return a result similar to the request `rpa.fields.getSettings`.

{% include [Example Notes](../../../../_includes/examples.md) %}