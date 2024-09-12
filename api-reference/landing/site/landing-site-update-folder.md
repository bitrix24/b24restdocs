# Change Folder in the Site landing.site.updateFolder

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
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

The method `landing.site.updateFolder` changes the folder in the site.

## Parameters

#|
|| **Parameter** | **Description** | **Version** ||
|| **siteId**
[`unknown`](../../data-types.md) | Site identifier.

{% note warning %}

Write access permission is required for the specified site.

{% endnote %}

 | ||
|| **folderId**
[`unknown`](../../data-types.md) | Folder identifier in the site. | ||
|| **fields**
[`unknown`](../../data-types.md) | Folder fields: 
- ACTIVE – folder activity (Y/N). By default, it is created as inactive;
- TITLE – title (name) of the folder;
- INDEX_ID – identifier of the page within the folder that needs to be made the index page of the folder;
- CODE – symbolic code of the folder (part of the URL of the folder page). By default, it is transliterated from the folder name. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.updateFolder',
    {
        siteId: 1817,
        folderId: 736,
        fields: {
            TITLE: 'Updated Folder'
        }
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