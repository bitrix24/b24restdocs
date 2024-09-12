# Rename file disk.file.rename

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing (there should be three examples - curl, js, php)
- response in case of error is missing

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.file.rename` renames a file.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier. ||
|| **newName**
[`unknown`](../../data-types.md) | New name. ||
|#

## Example

```js
BX24.callMethod(
    "disk.file.rename",
    {
        id: 10,
        newName: 'Newname for file.png'
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

## Response in case of success

> 200 OK

The response has the same structure as in [disk.file.get](./disk-file-get.md).