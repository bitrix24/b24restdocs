# Create a new comment in the timeline rpa.comment.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameters are not specified
- examples are missing
- response in case of error is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.comment.add` will create a new comment in the timeline of the item with the identifier itemId of the process with the identifier typeId.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **itemId** 
[`number`](../../../data-types.md) | Identifier of the item. ||
|| **fields** 
[`array`](../../../data-types.md) | Comment fields. ||
|#

## Parameter fields

#|
|| **Parameter** | **Description** ||
|| **description** | Description of the entry (HTML and BB-code can be used). ||
|| **files** | Array of attached files, where each element is an array containing the name and base64 encoded content. ||
|#

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
                [
                    "document.pdf", "...base64_decoded_content..."
                ]
            ]    
        }
    }
    ```

{% endlist %}

{% include [Footnote about examples](../../../../_includes/examples.md) %}

## Response in case of success

> 200 OK

```json
{
    "comment": {
        "id": 350,
        "createdTime": "2020-03-27T16:00:59+02:00",
        "isFixed": false,
        "typeId": 24,
        "itemId": 10,
        "action": "comment",
        "description": "Mention of user with id 1 ",
        "userId": 1,
        "title": "Comment",
        "data": {
            "files": [
                15
            ]
        },
        "createdTimestamp": 1585317659000,
        "htmlDescription": "Mention of user with id 1 <a class=\"blog-p-user-name\" id=\"bp_K6r6vvp7\" href=\"/company/personal/user/1/\" bx-tooltip-user-id=\"1\">Anton Gorbylev</a> ",
        "textDescription": "Mention of user with id 1 Anton",
        "users": {
            "1": {
                "id": "1",
                "name": "Anton",
                "secondName": "",
                "lastName": "",
                "title": null,
                "workPosition": "",
                "fullName": "Anton",
                "link": "/company/personal/user/1/"
            }
        }
    }
}
```