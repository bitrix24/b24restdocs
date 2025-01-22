# Update Timeline Entry rpa.comment.update

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- the mandatory parameters are not specified
- examples are missing
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.comment.update` will update the timeline entry with the identifier id.

This method allows you to modify only the fields title and description.

#|
|| **Name**
`type` | **Description** ||
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

To add a new file, you need to pass a list as the entry for the old file, where the key id will be the identifier of the file attached to this comment.

To upload new files, you also need to pass an array with the name and content of the file in base64.

## Example

{% list tabs %}

- JS

    ```json
    {
        "typeId": 24,
        "itemId": 10,
        "fields": {
            "description": "Mention of user with id 1 ",
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

{% endlist %}

{% include [Footnote on examples](../../../../_includes/examples.md) %}