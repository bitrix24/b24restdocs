# Copy Folder to Specified Folder disk.folder.copyto

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response provided
- detailed success response is needed

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.copyto` copies a folder to the specified folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|| **targetFolderId**
[`unknown`](../../data-types.md) | Identifier of the folder to which the copy is made. ||
|#

## Example

```js
BX24.callMethod(
    "disk.folder.copyto",
    {
        id: 8,
        targetFolderId: 22081990
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

The response has the same structure as in [disk.folder.get](./disk-folder-get.md).