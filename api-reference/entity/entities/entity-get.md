# Get Storage Parameters or List of All Storages entity.get

{% note warning "We are still updating this page" %}

Some data may be missing — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- required parameter specifications are missing
- examples are absent
- response in case of error is missing

{% endnote %}

{% endif %}

> Scope: [`entity`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The `entity.get` method retrieves the storage parameters or a list of all storages of the application.

## Parameters

#|
|| **Parameter** | **Description** ||
|| **ENTITY**
[`string`](../../data-types.md) | String identifier of the required storage. ||
|#

{% include [Note on parameters](../../../_includes/required.md) %}

## Examples

{% list tabs %}

- JS

    ```javascript
    BX24.callMethod('entity.get');
    ```

- HTTP

    ```http
    https://my.bitrix24.com/rest/entity.get.json?auth=59efe32d01c0e9dc5732e8dfa68a4baa
    ```

{% endlist %}

Example of correctly retrieving a list of all available storages:

{% list tabs %}

- JS

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

{% endlist %}

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