# Update Timeline Entry rpa.comment.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- Required parameters are not specified
- Examples are missing
- Success response is missing
- Error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.comment.update` will update the timeline entry with the identifier id.

This method allows you to change only the fields title and description.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id** 
[`number`](../../../data-types.md) | Identifier of the comment. ||
|| **fields** 
[`array`](../../../data-types.md) | Entry fields. ||
|#

## Parameter fields

#|
|| **Parameter** | **Description** ||
|| **description** | Description of the entry (HTML and BB-code can be used). ||
|| **files** | Array of attached files, where each element is an array containing the name and base64 encoded content. ||
|#

{% note warning %}

This method allows you to change only those comments that were added by the same user.

{% endnote %}

To add a new file, you need to provide a list where the key id will be the identifier of the file attached to this comment.

To upload new files, you must also provide an array with the name and content of the file in base64.

## Example

```json
{
    "typeId": 24,
    "itemId": 10,
    "fields": {
        "description": "Mentioning user with id 1 ",
        "files": [
            {
                "id": 15
            },
            [
                "another_document.pdf", "...base64_decoded_content..."
            ]
        ]
    }
}
```

{% include [Examples Note](../../../../_includes/examples.md) %}