# Get Process Information by ID rpa.type.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- examples are missing
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`rpa`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `rpa.type.get` returns information about a process by its ID.

#|
|| **Parameter** / **Type** | **Description** ||
|| **id**^*^ 
[`number`](../../../data-types.md) | Process ID. ||
|#

{% include [Parameter Notes](../../../../_includes/required.md) %}

## Successful Response

> 200 OK

```json
{
    "type": {
        "id":1,
        "title":"Process Name",
        "image":"list",
        "createdBy":1,
        "settings":[],
        "permissions":[
            {
                "id":"1",
                "entity":"TYPE",
                "entityId":"1",
                "accessCode":"UA",
                "action":"ITEMS_CREATE",
                "permission":"X"
            }
        ]
    }
}
```

- `title` - name of the process
- `image` - this is the icon identifier
- `createdBy` - ID of the user who created the process
- `settings` - set of process settings
- `permissions` - set of access permissions for this process