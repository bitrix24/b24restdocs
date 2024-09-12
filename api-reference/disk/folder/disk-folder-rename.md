# Rename Folder disk.folder.rename

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.rename` renames a folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|| **newName**
[`unknown`](../../data-types.md) | New name. ||
|#

## Example

```js
BX24.callMethod(
    "disk.folder.rename",
    {
        id: 8,
        newName: 'New name'
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
{% include [Note on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

The response has the same structure as in [disk.folder.get](./disk-folder-get.md).