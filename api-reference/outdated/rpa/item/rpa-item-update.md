# Update Process Element rpa.item.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning "DEPRECATED" %}

The development of this method has been halted. Please use [Smart scripts](../../../crm/universal/user-defined-object-types/index.md) as an alternative functionality.

{% endnote %}

This method updates the element with the identifier `id` in the process with the identifier `typeId`.

## Method Parameters

{% include [Parameter Note](../../../../_includes/required.md) %}

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`integer`](../../../data-types.md) | Process identifier ||
|| **id** 
[`integer`](../../../data-types.md) | Element identifier ||
|| **fields*** 
[`object`](../../../data-types.md) | Object containing values for custom [fields](#fields) of the element ||
|#

## Parameter fields {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **stageId** 
[`integer`](../../../data-types.md) | Stage identifier ||
|| **UF_RPA_...** 
[`any`](../../../data-types.md) | Values for custom fields. The format for passing values depends on the field type ||
|#

## Code Examples

1. Upload a new file to replace the old one (non-multiple field)

    To replace a file in a non-multiple field, simply upload the new file. The old one will be automatically deleted.

    {% list tabs %}

    - JS

        ```json
        {
            "fields": {
                "UF_RPA_1_1585069397": [
                    "myfile.pdf", "...base64_encoded_file_content..."
                ]
            }
        }
        ```

    {% endlist %}

2. Remove the value of a custom file field

    To do this, simply pass an empty string `''` instead of the value.

3. Keep the value of a non-multiple file field unchanged

    The simplest option is to not include the key for this field in `fields`. However, if you need to pass it without changing it, you should provide a list where the key `id` contains the file identifier.

    {% list tabs %}

    - JS

        ```json
        {
            "fields": {
                "UF_RPA_1_1585069397": {
                    "id": 433    
                }
            }
        }
        ```

    {% endlist %}

    {% note warning %}

    If a value different from the current one is passed in `id`, the field value will be reset and the file will be deleted.

    {% endnote %}

4. Working with a multiple file field

    The value of a multiple field is an array. Each element of the array follows the same rules as for non-multiple values.

5. Partial overwrite of a multiple file field value

    For example, currently the multiple file field contains the values `[12, 255, 44]`.

    You need to keep files `12` and `44`, and upload a new file instead of `255`.

    The request should look as follows:

    {% list tabs %}

    - JS

        ```json
        {
            "fields": {
                "UF_RPA_1_1585069397": [
                    {
                        "id": 12
                    },
                    {
                        "id": 44
                    },
                    [
                        "myNewFile.pdf",
                        "...base64_encoded_file_content..."
                    ]
                ]
            }
        }
        ```

    {% endlist %}

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-item-add.md)
- [{#T}](./rpa-item-get.md)
- [{#T}](./rpa-item-get-tasks.md)
- [{#T}](./rpa-item-list.md)
- [{#T}](./rpa-item-delete.md)