# Move a folder to a specified folder disk.folder.moveto

{% if build == 'dev' %}

{% note alert "TO-DO _not uploaded to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response is provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will add it soon

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.moveto` copies a folder to the specified folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|| **targetFolderId**
[`unknown`](../../data-types.md) | Identifier of the target folder. ||
|#

## Example

```js
BX24.callMethod(
    "disk.folder.moveto",
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

## Response on success

> 200 OK

The response has the same structure as in [disk.folder.get](./disk-folder-get.md).