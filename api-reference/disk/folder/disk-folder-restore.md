# Restore Folder from Trash disk.folder.restore

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
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

The method `disk.folder.restore` restores a folder from the trash.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|#

## Example

```js
BX24.callMethod(
    "disk.folder.restore",
    {
        id: 8
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

## Success Response

> 200 OK

The response has the same structure as in [disk.folder.get](./disk-folder-get.md).