# Rename Folder disk.folder.rename

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

The method `disk.folder.rename` renames a folder.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Folder identifier. ||
|| **newName**
[`unknown`](../../data-types.md) | New name. ||
|#

## Example

{% list tabs %}

- JS

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

{% endlist %}

{% include [Footnote on examples](../../../_includes/examples.md) %}

## Success Response

> 200 OK

The response has the same structure as in [disk.folder.get](./disk-folder-get.md).