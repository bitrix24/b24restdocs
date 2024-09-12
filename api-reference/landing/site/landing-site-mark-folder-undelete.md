# Restore Folder from Trash landing.site.markFolderUnDelete

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- parameter requirements are not specified
- examples are missing
- success response is missing
- error response is missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.markFolderUnDelete` marks a folder as not deleted (restores it from the trash).

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. Access permissions for the folder's site must include deletion rights. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.markFolderUnDelete',
    {
        id: 737
    },
    function(result)
    {
        if(result.error())
        {
            console.error(result.error());
        }
        else
        {
            console.info(result.data());
        }
    }
);
```

{% include [Footnote on examples](../../../_includes/examples.md) %}