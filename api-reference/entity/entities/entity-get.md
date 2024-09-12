# Get Storage Parameters or List of All Storages entity.get

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

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.get` method retrieves the storage parameters or a list of all storages in the application.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**
[`string`](../../data-types.md) | String identifier of the required storage. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

```javascript
BX24.callMethod('entity.get');
```

Request
```http
https://my.bitrix24.com/rest/entity.get.json?auth=59efe32d01c0e9dc5732e8dfa68a4baa
```

Example of successfully retrieving a list of all available storages:

```javascript
BX24.callMethod(
    "entity.get",
    {},
    function(result)
    {
        if(result.error())
            console.error(result.error());
        else
        {
            console.info("List of created storages:", result.data());
        }
    }
);
```

{% include [Note on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK
```json
{"result":
    [
        {
            "ENTITY":"dish",
            "NAME":"Dishes"
        },
        {
            "ENTITY":"menu",
            "NAME":"Menu"
        }
    ]
}
```