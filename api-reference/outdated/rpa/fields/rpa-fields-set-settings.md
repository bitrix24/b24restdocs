# Set Full Field Visibility Settings rpa.fields.setSettings

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method sets the full set of field visibility settings for the stage with the identifier `stageId` of the process with the identifier `typeId`.

## Method Parameters

{% include [Parameter Notes](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId*** 
[`number`](../../../data-types.md) | Identifier of the process ||
|| **stageId** 
[`number`](../../../data-types.md) | Identifier of the stage.

Defaults to `0`, meaning — general settings ||
|| **fields*** 
[`object`](../../../data-types.md) | Array of field visibility settings.

If an empty `fields` is passed, all settings will be cleared ||
|#

## Code Examples

{% include [Example Notes](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'rpa.comment.add',
        {
            "typeId": 1,
            "fields": {
                "kanban": [
                    "createdBy",
                    "UF_RPA_1_NAME"
                ]
            }
        },
        function(result) {
            console.log('response', result.answer);
            if(result.error())
                alert("Error: " + result.error());
            else
            console.log(result.data());
        }
    )
    ```

{% endlist %}

## Response Handling

The method will return a result similar to the request [rpa.fields.getSettings](./rpa-fields-get-settings.md).

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-fields-get-settings.md)
- [{#T}](./rpa-fields-set-visibility-settings.md)