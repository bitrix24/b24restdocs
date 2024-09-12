# Copy File to Specified Folder disk.file.copyto

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

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

The method `disk.file.copyto` copies a file to the specified folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | File identifier. ||
|| **targetFolderId**
[`unknown`](../../data-types.md) | Identifier of the folder where the copy will be made. ||
|#

## Example

```js
BX24.callMethod(
    "disk.file.copyto",
    {
        id: 10,
        targetFolderId: 226
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

The response has the same structure as in [disk.file.get](./disk-file-get.md).