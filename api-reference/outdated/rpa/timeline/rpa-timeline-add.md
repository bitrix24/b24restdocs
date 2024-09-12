# Create a New Timeline Entry rpa.timeline.add

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.timeline.add` will create a new timeline entry for the entity with the identifier itemId of the process with the identifier typeId.

This method allows modification of only the title and description fields.

#|
|| **Parameter** / **Type** | **Description** ||
|| **typeId** 
[`number`](../../../data-types.md) | Identifier of the process. ||
|| **itemId** 
[`number`](../../../data-types.md) | Identifier of the entity. ||
|| **fields** 
[`array`](../../../data-types.md) | Entry fields. ||
|#

## Fields Parameters

#|
|| **Parameter** | **Description** ||
|| **title** | Title of the entry. ||
|| **description** | Description of the entry (HTML can be used). ||
|#

## Example

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
                "name": "Anthony",
                "secondName": "",
                "lastName": "",
                "title": null,
                "workPosition": "",
                "fullName": "Anthony",
                "link": "/company/personal/user/1/"
            }
        }
    }
}
```

{% include [Footnote on examples](../../../../_includes/examples.md) %}