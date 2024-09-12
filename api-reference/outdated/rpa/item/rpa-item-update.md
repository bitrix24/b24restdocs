# Update Process Element rpa.item.update

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is absent
- Error response is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.item.update` updates the element with the identifier id of the process with the identifier typeId.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the element. ||
|| **fields**^*^ 
[`array`](../../../data-types.md) | Values of the custom fields of the element. ||
|#

{% include [Parameter Note](../../../../_includes/required.md) %}

## fields Parameters

#|
|| **Parameter** | **Description** ||
|| **stageId** | Identifier of the stage. ||
|| **UF_RPA_...** | Values of custom fields. ||
|#

## Examples

**Upload a new file instead of the old one (single field)**

To replace a file in a single field, simply upload the new file. The old one will be automatically deleted.

```json
{
    "fields": {
        "UF_RPA_1_1585069397": [
             "myfile.pdf", "...base64_encoded_file_content..."
        ]
    }
}
```

**Remove the value of a file-type custom field**

To do this, simply pass an empty string (`''`) instead of the value.

**Leave the value of a single file-type field unchanged**

The simplest option is to not add a key for this field in `fields`. But if you need to pass it and not change it, then you should pass a list where the key `id` will be the identifier of the file.

```json
{
    "fields": {
        "UF_RPA_1_1585069397": {
            "id": 433    
        }
    }
}
```
{% note warning %}

If a value different from the current one is passed in `id`, the field value will be reset and the file will be deleted.

{% endnote %}

**Working with a multiple file-type field**

The value of a multiple field is an array. Each element of the array follows the same rules as for single values.

**Partial overwrite of a multiple file-type field value**

For example, currently, the multiple file-type field contains the values `[12, 255, 44]`.

You need to keep files `12` and `44`, and upload a new one instead of `255`.

The request should look as follows:

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

{% include [Examples Note](../../../../_includes/examples.md) %}