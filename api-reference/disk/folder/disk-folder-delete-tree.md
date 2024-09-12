# Delete Folder and All Child Elements Permanently disk.folder.deletetree

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not indicated
- examples are missing (there should be three examples - curl, js, php)
- no error response provided

{% endnote %}

{% endif %}

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly

{% endnote %}

> Scope: [`disk`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `disk.folder.deletetree` permanently deletes a folder and all its child elements.

## Parameters

#|
||  **Parameter** / **Type**| **Description** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. ||
|#

## Example

```js
BX24.callMethod(
    "disk.folder.deletetree",
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
{% include [Footnote on examples](../../../_includes/examples.md) %}

## Response on Success

> 200 OK

```json
"result": true
```
Successful deletion of the file.