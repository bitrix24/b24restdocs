# Change Visibility Settings for rpa.fields.setVisibilitySettings

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- missing fields table
- missing examples
- missing success response
- missing error response

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.fields.setVisibilitySettings` changes the visibility settings of fields for the process with the identifier typeId at the stage with the identifier stageId. Other settings remain unchanged.

This method should be used when you need to change the visibility settings for only one type.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId**^*^ 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **visibility**^*^ 
[`string`](../../../data-types.md) | Identifier of the visibility for which the settings are being changed. ||
|| **stageId** 
[`number`](../../../data-types.md) | Identifier of the stage. Default is 0 (general settings). ||
|| **fields**^*^ 
[`array`](../../../data-types.md) | Array of fields for which the setting needs to be changed. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Example

{% list tabs %}

- JS

    ```json
    {
        "typeId": 1,
        "visibility": "kanban",
        "fields": [
            "createdBy", 
            "UF_RPA_1_NAME"
        ]
    }
    ```

{% endlist %}

The method will return a result similar to the request `rpa.fields.getSettings`.

{% include [Example Notes](../../../../_includes/examples.md) %}