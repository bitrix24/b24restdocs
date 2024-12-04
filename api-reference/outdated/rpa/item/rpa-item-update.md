# Update Process Element rpa.item.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- examples are absent
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.update` updates the element with the identifier id of the process with the identifier typeId.

#| 
|| **Parameter** / **Type** | **Description** || 
|| **typeId** 
[`number`](../../../data-types.md) | Process identifier. || 
|| **id** 
[`number`](../../../data-types.md) | Element identifier. || 
|| **fields**^*^ 
[`array`](../../../data-types.md) | Values of the element's custom fields. || 
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## fields Parameters

#| 
|| **Parameter** | **Description** || 
|| **stageId** | Stage identifier. || 
|| **UF_RPA_...** | Values of custom fields. || 
|#

## Examples

**Upload a new file instead of the old one (non-multiple field)**

To replace a file in a non-multiple field, simply upload a new file. The old one will be automatically deleted.

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

**Remove the value of a file-type custom field**

To do this, simply pass an empty string (`''`) instead of the value.

**Leave the value of a non-multiple file-type field unchanged**

The simplest option is to not add the key for this field in `fields`. However, if you need to pass it without changing it, you should pass a list where the `id` key contains the file identifier.

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

**Working with a multiple file-type field**

The value of a multiple field is an array. Each element of the array follows the same rules as for non-multiple values.

**Partial overwrite of a multiple file-type field value**

For example, currently, the multiple file-type field contains the value `[12, 255, 44]`.

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

{% include [Example Notes](../../../../_includes/examples.md) %}