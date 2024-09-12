# Mark Folder as Deleted landing.site.markFolderDelete

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

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

The method `landing.site.markFolderDelete` marks a folder as deleted (moved to the trash).

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **id**
[`unknown`](../../data-types.md) | Identifier of the folder. Access permissions for deleting the folder must be granted. | ||
|#

## Example

```js
BX24.callMethod(
    'landing.site.markFolderDelete',
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

{% include [Example Note](../../../_includes/examples.md) %}