# Create a new timeline entry rpa.timeline.add

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.timeline.add` will create a new timeline entry for the item with the identifier itemId of the process with the identifier typeId.

This method allows modifying only the fields title and description.

#| 
|| **Parameter** / **Type** | **Description** || 
|| **typeId** 
[`number`](../../../data-types.md) | Process identifier. || 
|| **itemId** 
[`number`](../../../data-types.md) | Item identifier. || 
|| **fields** 
[`array`](../../../data-types.md) | Entry fields. || 
|#

## Fields parameters

#| 
|| **Parameter** | **Description** || 
|| **title** | Entry title. || 
|| **description** | Entry description (HTML can be used). || 
|#

## Example

{% list tabs %}

- JS

    ```json
    {
        "timeline": {
            "id": 325,
            "typeId": 24,
            "itemId": 10,
            "createdTime": "2020-03-26T21:55:25+02:00",
            "userId": 1,
            "title": "rest update",
            "description": "<h5>small header</h5>",
            "action": false,
            "isFixed": false,
            "data": {
                "scope": "rest"
            },
            "createdTimestamp": 1585252525000,
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

{% endlist %}

{% include [Note on examples](../../../../_includes/examples.md) %}