# Get Folders of the Site landing.site.getFolders

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- adjustments needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`landing`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `landing.site.getFolders` retrieves the folders of the site.

## Parameters

#|
|| **Parameter** | **Description** | **Available since** ||
|| **siteId**
[`unknown`](../../data-types.md) | The identifier of the site. 

{% note warning %}

Write permissions for the specified site are required. 

{% endnote %}

| ||



|| **filter**
[`unknown`](../../data-types.md) | Optional filter. Can accept the following fields:
- ACTIVE – folder activity (Y/N). By default, it is created as inactive;
- DELETED – folder deleted (Y/N). By default, non-deleted folders are returned;
- PARENT_ID – identifier of the parent folder;
- TITLE – title of the folder;
- INDEX_ID – identifier of the folder's index page;
- CODE – symbolic code of the folder;
- CREATED_BY_ID – identifier of the user who created the folder;
- MODIFIED_BY_ID – identifier of the user who modified the folder; | ||
|#

## Examples

```js
BX24.callMethod(
    'landing.site.getFolders',
    {
        siteId: 1817,
        filter: {
            TITLE: 'Modified Folder'
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