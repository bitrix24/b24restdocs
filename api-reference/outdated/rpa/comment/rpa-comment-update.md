# Update Timeline Entry rpa.comment.update

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method updates the timeline entry with the identifier `id`. It only updates the `title` and `description` fields.

The method allows changes only to those comments that were added by the same user.

## Method Parameters

#|
|| **Name**
`type` | **Description** ||
|| **id** 
[`integer`](../../../data-types.md) | Identifier of the comment ||
|| **fields** 
[`object`](../../../data-types.md) | Object describing the [fields](#fields) of the comment ||
|#

## Fields Parameter {#fields}

#|
|| **Name**
`type` | **Description** ||
|| **description** 
[`integer`](../../../data-types.md) | Description of the entry. HTML and BB-code formatting can be used ||
|| **files** 
[`integer`](../../../data-types.md) | Array of attached files. Each element is an array containing the name and content encoded in base64

To add a new file, you must provide a list as a record of the old file, where the key `id` will be the identifier of the file attached to this comment.

To upload new files, you must also provide an array with the name and content of the file in base64. ||
|#

## Code Examples

{% include [Footnote on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        'rpa.comment.add',
        {
            "typeId": 24,
            "itemId": 10,
            "fields": {
                "description": "Mention of user with id 1 ",
                "files": [
                    {
                        "id": 15 // identifier of the old file
                    },
                    [
                        "another_document.pdf", "...base64_decoded_content..."
                    ]
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

## Continue Learning 

- [{#T}](./index.md)
- [{#T}](./rpa-comment-add.md)
- [{#T}](./rpa-comment-delete.md)