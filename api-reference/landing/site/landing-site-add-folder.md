# Add Folder to the Site landing.site.addFolder

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

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

The method `landing.site.addFolder` adds a folder to the site.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../data-types.md) | Identifier of the site. 

{% note warning %}

Write permissions are required for the specified site.

{% endnote %}

 | ||

|| **fields**
[`unknown`](../../data-types.md) | Folder fields: 
- ACTIVE – folder activity (Y/N). By default, it is created as inactive;
- TITLE – title (name) of the folder; 
- CODE – symbolic code of the folder (part of the folder page URL). By default, it is transliterated from the folder name. | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.addFolder',
    {
        siteId: 1817,
        fields: {
            TITLE: 'New Folder'
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