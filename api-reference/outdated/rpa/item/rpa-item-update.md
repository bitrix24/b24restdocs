# Update Process Element rpa.item.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.update` updates the element with the identifier id of the process with the identifier typeId.

#|
|| **Name**
`type` | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the element. ||
|| **fields**^*^ 
[`array`](../../../data-types.md) | Values of the custom fields of the element. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## Parameters fields

#|
|| **Parameter** | **Description** ||
|| **stageId** | Identifier of the stage. ||
|| **UF_RPA_...** | Values of custom fields. ||
|#

## Examples

**Upload a new file instead of the old one (non-multiple field)**

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

**Remove the value of a custom file field**

To do this, simply pass an empty string (`''`) instead of the value.

**Leave the value of a non-multiple file field unchanged**

The simplest option is not to add a key for this field in `fields`. However, if you need to pass it without changing, you should pass a list where the key `id` will be the identifier of the file.

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

**Working with a multiple file field**

The value of a multiple field is an array. Each element of the array follows the same rules as for non-multiple values.

**Partial overwrite of a multiple file field value**

For example, currently, the multiple file field contains the values `[12, 255, 44]`.

You need to keep files `12` and `44`, and upload a new one instead of `255`.

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

{% include [Examples Note](../../../../_includes/examples.md) %}