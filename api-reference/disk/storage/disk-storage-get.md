# Get storage description by its identifier disk.storage.get

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is absent

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.get` returns the storage by its identifier.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Storage identifier. ||
|#

## Example

{% list tabs %}

- JS

    ```js
    BX24.callMethod(
        "disk.storage.get",
        {id: 2},
        function (result)
        {
            if (result.error())
                console.error(result.error());
            else
                console.dir(result.data());
        }
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../../_includes/examples.md) %}

## Response in case of success

> 200 OK

```json
"result": {
    "ID": "2", //identifier
    "NAME": "Marketing and Advertising", //name
    "CODE": null, //symbolic code
    "MODULE_ID": "disk",
    "ENTITY_TYPE": "group", //entity type (see disk.storage.gettypes)
    "ENTITY_ID": "1", //entity identifier
    "ROOT_OBJECT_ID": "2" //root folder identifier
}
```