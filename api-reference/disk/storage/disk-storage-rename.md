# Rename Storage disk.storage.rename

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- no error response provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.storage.rename` renames the storage. Only the application storage can be renamed (see [disk.storage.getforapp](./disk-storage-get-for-app.md)).

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Storage identifier. ||
|| **newName**
[`unknown`](../../data-types.md) | New name. ||
|#

## Example

```js
BX24.callMethod(
    "disk.storage.rename",
    {
        id: 2,
        newName: 'New name for storage'
    },
    function (result)
    {
        if (result.error())
            console.error(result.error());
        else
            console.dir(result.data());
    }
);
```
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Success Response

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