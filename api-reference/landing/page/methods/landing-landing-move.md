# Move Page to Another Site / Folder landing.landing.move

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- parameter types not specified
- parameter requirements not indicated
- examples missing
- success response missing
- error response missing

{% endnote %}

{% endif %}

> Scope: [`landing`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.landing.move` transfers a page to another site and/or folder.

## Parameters

#|
|| **Parameters** | **Description** | **Available since** ||
|| **lid**
[`unknown`](../../../data-types.md) | The identifier of the page to be moved. ||
|| **toSiteId**
[`unknown`](../../../data-types.md) | The identifier of the site to which the page should be moved. Write permissions for this site are required. ||
|| **toFolderId**
[`unknown`](../../../data-types.md) | The identifier of the site folder to which the page should be moved. The folder must be within the specified site. To move to the root of the site, this parameter should be omitted. (To move within the current site - omit the toSiteId parameter as well). ||
|#

## Examples

```js
BX24.callMethod(
    'landing.landing.move',
    {
        lid: 11262,
        toSiteId: 1817,
        toFolderId: 737
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

{% include [Footnote on examples](../../../../_includes/examples.md) %}