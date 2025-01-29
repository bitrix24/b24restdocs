# Change Field Visibility Settings rpa.fields.setVisibilitySettings

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the `visibility` settings of `fields` for the process with the identifier `typeId` at the stage with the identifier `stageId`. Other settings remain unchanged.

The method should be used when you need to change the visibility settings for only one type.

## Method Parameters

{% include [Footnote about parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId***  
[`integer`](../../../data-types.md) | Identifier of the process ||
|| **visibility***  
[`string`](../../../data-types.md) | Identifier of the visibility for which the settings are being changed ||
|| **stageId**  
[`integer`](../../../data-types.md) | Identifier of the stage.

Defaults to `0`, meaning — general settings ||
|| **fields***  
[`array`](../../../data-types.md) | Array of fields for which the setting needs to be changed ||
|#

## Code Examples

{% include [Footnote about examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'rpa.comment.add',
        {
            "typeId": 1,
            "visibility": "kanban",
            "fields": [
                "createdBy", 
                "UF_RPA_1_NAME"
            ]
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
- [{#T}](./rpa-fields-set-settings.md)